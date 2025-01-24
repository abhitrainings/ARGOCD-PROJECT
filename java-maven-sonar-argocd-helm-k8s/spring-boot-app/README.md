# End-to-End CI/CD Pipeline for Java Application

Welcome to the **Java CI/CD Pipeline Adventure!** This README serves as your guide to setting up an end-to-end Jenkins pipeline for a Java application using **SonarQube**, **Argo CD**, **Helm**, and **Kubernetes**. Let’s embark on this journey with some fun and clear steps—because deploying apps shouldn't feel like climbing Mount Doom! 🌋

---

## 🚀 Prerequisites
Before we dive into the pipeline, ensure you have the following:
- Java application code hosted on a Git repository.
- A Jenkins server up and running.
- Access to a Kubernetes cluster.
- Helm package manager installed.
- Argo CD deployed on the Kubernetes cluster.

---

## 🛠️ Step-by-Step Pipeline Setup

### 1. Install Necessary Jenkins Plugins
Your Jenkins needs some superpowers to handle this pipeline. Install the following plugins:
1. **Git Plugin**
2. **Maven Integration Plugin**
3. **Pipeline Plugin**
4. **Kubernetes Continuous Deploy Plugin**

### 2. Create a New Jenkins Pipeline
1. Log in to Jenkins and create a new **pipeline job**.
2. Configure the job with the Git repository URL where your Java application resides.
3. Add a `Jenkinsfile` to your Git repository to define the pipeline stages.

### 3. Define the Pipeline Stages
Here’s the breakdown of pipeline stages:
1. **Checkout**: Pull the source code from Git.
2. **Build**: Use Maven to build the Java application.
3. **Unit Tests**: Execute unit tests with JUnit and Mockito.
4. **Code Analysis**: Run SonarQube analysis to ensure code quality.
5. **Package**: Package the application into a JAR file.
6. **Deploy to Test**: Deploy the application to a test environment using Helm.
7. **User Acceptance Tests (UAT)**: Run automated UAT on the deployed application.
8. **Promote to Production**: Deploy the application to production using Argo CD.

### 4. Configure Jenkins Pipeline Stages
#### Stage 1: **Checkout**
- Use the **Git Plugin** to fetch the source code from the repository.
#### Stage 2: **Build**
- Use the **Maven Integration Plugin** to compile the Java application.
#### Stage 3: **Unit Tests**
- Integrate JUnit and Mockito to run your unit tests and ensure your app isn't an accidental bug magnet. 🐛
#### Stage 4: **Code Analysis**
- Leverage **SonarQube Plugin** to assess your code quality and keep those pesky code smells at bay.
#### Stage 5: **Package**
- Use Maven to package your application into a shiny JAR file. “Your app, now in portable form!” 📦
#### Stage 6: **Deploy to Test**
- Use the **Kubernetes Continuous Deploy Plugin** to deploy to the test environment with Helm.
#### Stage 7: **UAT**
- Run automated user acceptance tests using frameworks like Selenium.
#### Stage 8: **Promote to Production**
- Call **Argo CD** to roll out the final deployment to production.

### 5. Set Up Argo CD
1. Install **Argo CD** on your Kubernetes cluster (it’s like Kubernetes’s best friend). 🤝
2. Configure Argo CD to track changes in your Helm charts and Kubernetes manifests.
3. Create a Helm chart for your Java application, including:
   - Kubernetes manifests
   - Helm values
4. Push the Helm chart to a Git repository that Argo CD can monitor.

### 6. Integrate Argo CD with Jenkins
1. Add the Argo CD API token to Jenkins credentials (because security matters!). 🔑
2. Update the Jenkins pipeline to include a deployment stage for Argo CD.

### 7. Run the Pipeline
1. Trigger the Jenkins pipeline to kick off the CI/CD process.
2. Monitor each stage—and don't forget to grab popcorn while watching the magic happen. 🍿
3. Address any hiccups along the way (debugging = detective work 🕵️).



---

## 🤝 Conclusion
By the end of this setup, you’ll have an automated CI/CD pipeline that takes your Java application from code to production. Tools like Jenkins, SonarQube, Helm, and Argo CD will work together to make deployments smoother than ever. And yes, the world is a better place when apps deploy flawlessly! 🌍

Now go ahead, set it up, and make your pipeline dreams a reality! 🎉





# CI/CD Pipeline Implementation for Spring Boot Java Application

Welcome to the sequel of our CI/CD pipeline setup! If the first document was the trailer, this is the blockbuster movie. 🎥 Grab your DevOps popcorn and get ready to execute every thrilling step in setting up and running the CI/CD pipeline for your **Spring Boot Java Application**. With a sprinkle of fun and crazy, let's get cracking! 💥

---

## 🎯 About the Application

This is a simple **Spring Boot** application built using **Maven**. It follows the **MVC architecture** where controllers return a page with `title` and `message` attributes to the view.

Dependencies? All handled elegantly with the `pom.xml` file. Think of it as the secret sauce in this recipe. 🌟

---

## 🔧 Execute Locally and Access via Browser

### 1. Clone the Repository

First things first, get your hands on the repo:

```bash
git clone https://github.com/abhitrainings/ARGOCD-PROJECT/tree/main/java-maven-sonar-argocd-helm-k8s
cd java-maven-sonar-argocd-helm-k8s/sprint-boot-app
```

### 2. Build the Application

Time to invoke Maven magic:

```bash
mvn clean package
```

This command packages the application and stores the artifacts in the `target/` directory.

### 3. Run Locally

With **Java 11** installed, you can execute the artifact directly:

```bash
java -jar target/spring-boot-web.jar
```

Access your app at [http://localhost:8080](http://localhost:8080). If it’s alive, congrats! You’ve just summoned your app into existence. 🧙

---

## 🛠️ Next Steps

### 1. Install Jenkins

#### Prerequisites

Create an EC2 instance with at least **8GB RAM**. Then, install Java and Jenkins:

```bash
sudo apt update
sudo apt install openjdk-17-jre
```

Verify Java installation:

```bash
java -version
```

Next, install Jenkins:

```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \  
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \  
  https://pkg.jenkins.io/debian binary/ | sudo tee \  
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

### 2. Open Jenkins to External World

AWS blocks inbound traffic by default. To access Jenkins, open **port 8080** in the EC2 instance’s inbound traffic rules.

#### Login Steps

1. Retrieve the Jenkins admin password:

   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

2. Enter the admin password and click **Install Suggested Plugins**. You’re in! 🚪

---

### 3. Install Docker Pipeline Plugin

#### Steps

1. Log in to Jenkins.
2. Navigate to **Manage Jenkins > Manage Plugins**.
3. Search for **Docker Pipeline** in the Available tab.
4. Install the plugin and restart Jenkins.

### 4. Docker Slave Configuration

#### Install Docker

```bash
sudo apt update
sudo apt install docker.io
```

Grant Docker daemon permissions to Jenkins and Ubuntu users:

```bash
sudo su -
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Restart Jenkins:

```bash
http://<ec2-instance-public-ip>:8080/restart
```

**Congrats!** Your Docker agent configuration is now ready. 🚀

---

## 🦸 Configure SonarQube

Let’s make your code spotless:

1. Install SonarQube:

   ```bash
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
   ```

2. Access the SonarQube UI using your public IP and port.

3. Generate a token and add it to Jenkins credentials.

### 5. Update Jenkins Pipeline

Update the `Jenkinsfile` to include SonarQube URL and credentials.

Run the pipeline and ensure a Docker image gets pushed to Docker Hub. 🎉

---

## 🌀 Deploy with Argo CD

### 1. Install Argo CD

1. Deploy Argo CD in your Kubernetes cluster:

   ```bash
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

2. Expose the Argo CD service by modifying the service type to **NodePort**:

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: argocd-server
   spec:
     type: NodePort
   ```

3. Apply the manifest:

   ```bash
   kubectl apply -f argocd-service.yaml
   ```

4. Wait until all pods are in the **Running** state:

   ```bash
   kubectl get pods -n argocd
   ```

### 2. Access Argo CD

Retrieve the Argo CD admin password:

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```

Login at the Argo CD UI and create an application set. Configure it to monitor your Git repository and fetch the deployment manifests. When the application set is created, it will fetch the deployment manifests and deploy the application in the Kubernetes cluster under the specified namespace. This ensures that the application is deployed correctly. Whenever changes occur in the source code, a new Docker image is built and the manifest is updated. Argo CD detects these changes and automatically triggers the deployment in the Kubernetes cluster. This seamless workflow showcases how powerful and efficient this setup is! 🚀

---

## 🌟 Conclusion

Congratulations! You now have an end-to-end CI/CD pipeline to:

- Build and test your Spring Boot application.
- Analyze code with SonarQube.
- Package and deploy to Kubernetes with Argo CD.

**"Why did the pipeline go to production?" Because it passed all its tests and said, "Helm yeah!"** 🚀



