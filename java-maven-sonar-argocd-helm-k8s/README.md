
Spring Boot based Java web application
This is a simple Sprint Boot based Java application that can be built using Maven. Sprint Boot dependencies are handled using the pom.xml at the root directory of the repository.

This is a MVC architecture based application where controller returns a page with title and message attributes to the view.

Execute the application locally and access it using your browser
Checkout the repo and move to the directory

git clone https://github.com/abhitrainings/ARGOCD-PROJECT/tree/main/java-maven-sonar-argocd-helm-k8s
cd java-maven-sonar-argocd-helm-k8s/sprint-boot-app
Execute the Maven targets to generate the artifacts

mvn clean package
The above maven target stroes the artifacts to the target directory. You can either execute the artifact on your local machine (or) run it as a Docker container.


Execute locally (Java 11 needed) and access the application on http://localhost:8080
java -jar target/spring-boot-web.jar

Next Steps:

Install Jenkins.
Pre-Requisites:
Create ec2 instance with atleast 8gb ram
Java (JDK)
Run the below commands to install Java and Jenkins
Install Java

sudo apt update
sudo apt install openjdk-17-jre
Verify Java is Installed

java -version
Now, you can proceed with installing Jenkins

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules

After you login to Jenkins, - Run the command to copy the Jenkins Admin Password - sudo cat /var/lib/jenkins/secrets/initialAdminPassword - Enter the Administrator password

Click on Install suggested plugins

Install the Docker Pipeline plugin in Jenkins:
Log in to Jenkins.
Go to Manage Jenkins > Manage Plugins.
In the Available tab, search for "Docker Pipeline".
Select the plugin and click the Install button.
Restart Jenkins after the plugin is installed.

Docker Slave Configuration
Run the below command to Install Docker

sudo apt update
sudo apt install docker.io
Grant Jenkins user and Ubuntu user permission to docker deamon.
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
Once you are done with the above steps, it is better to restart Jenkins.

http://<ec2-instance-public-ip>:8080/restart
The docker agent configuration is now successful.
As we need to push image to dockerhub, we need to authenticate to it. So add dockerhub credentials in jenkins through manage credentilas.


Configure a Sonar Server in the same ec2 instance

sudo sh 
apt install unzip
adduser sonarqube
sudo su - sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
Take the public ip and port number and open the sonarqube UI. Generate Token and add it in jenkins credentials for authentication
Sonarqube URL has to be updated in Jenkinsfile
Once you are done with the above steps, it is better to restart Jenkins.

Run the Jenkins pipeline and you should see new docker image will be updated in Jenkinsfile and should see in dockerhub registry as well.
In sonarQube you can see project report.

Create either eks cluster or minikube, then install ArgoCD in it 
ARGOCD:
install argocd from operatorhub.io

We want it to be accessible outside, so change service from clusterip(default) to NodePort. Use below manifest to apply.

apiVersion: argoproj.io/v1alpha1
kind: ArgoCD
metadata:
  name: example-argocd
  labels:
    example: basic
spec:
  server:
    service:
       𝘁𝘆𝗽𝗲: 𝗡𝗼𝗱𝗲𝗣𝗼𝗿𝘁

kubectl apply -f argocd-basic.yaml

check pods and wait for 3 min and see all pods are in running state.

minikube service example-argocd-server
minikube service list 
Click on the URl and open the ARGOCD UI.
Then to login, you need to get password from secret and decode it.
kubectl get secrets 
kubectl edit secret example-argocd-cluster
get the secret and decode it using below command
echo secrettextyouhavecopied |  base64 -d 
copy output and login now.


Click on the URl and open the ARGOCD UI.
Create an application in Argocd with path of repo.
Application will fetch manifests from Git repo and deploys in respective k8's cluster and namespaces.
This is complete CI CD setup.


