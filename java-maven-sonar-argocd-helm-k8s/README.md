
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

Configure a Sonar Server locally

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


Run the below command to Install Docker

sudo apt update
sudo apt install docker.io
Grant Jenkins user and Ubuntu user permission to docker deamon.
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker

Once you are done with the above steps, it is better to restart Jenkins.

ARGOCD:
install argocd from operatorhub
check pods 
kubectl apply -f argocd-basic.yaml
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

