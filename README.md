hello








































pom.xml file example:


<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>
  <groupId>KMIT</groupId>
  <artifactId>ors-maven-webapp</artifactId>
  <version>1.0.0</version>
  <packaging>war</packaging>
  <name>ORS Maven Webapp</name>
  <url>http://localhost:8081</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.seleniumhq.selenium</groupId>
      <artifactId>selenium-java</artifactId>
      <version>4.11.0</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.commons</groupId>
      <artifactId>commons-lang3</artifactId>
      <version>3.12.0</version>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <!-- Clean plugin -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-clean-plugin</artifactId>
        <version>3.4.0</version>
      </plugin>

      <!-- Compiler plugin -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.11.0</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>

      <!-- Updated WAR plugin -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.3.2</version> <!-- UPDATED -->
      </plugin>

      <!-- Surefire plugin -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.3.0</version>
      </plugin>
    </plugins>
  </build>
</project>


generic git commands:


cd /path/to/your/project
git init
git remote add origin https://github.com/yourusername/your-private-repo
git add .
git commit -m "Initial commit"
git push -u origin master
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
git pull origin main
git remote remove origin
git remote rename origin upstream
git fetch <remote_name>
git remote set-url origin https://github.com/username/new-repository.git
git remote show origin
git branch                # Lists all branches or
git branch -a               # Lists all branches
git branch branch-name    # Creates a new branch
git branch -d <branch_name>    # Deletes a branch
git checkout branch-name  # Switches to an existing branch
git checkout -b new-branch # Creates and switches to a new branch
git checkout main         # Switch to the main branch
git merge branch-name     # Merge branch-name into main
git log


Generic docker commands:


docker pull redis
docker run --name my-redis -d redis
docker ps
docker exec -it my-redis redis-cli
docker stop my-redis


Dockerfile:


Write the Dockerfile FROM redis:latest CMD ["redis-server"]
docker build -t redisnew  .
docker run --name myredisnew -d redisnew
docker ps


docker-compose


version: "3.9"

services:
  web:
    image: nginx
    ports:
      - "8060:80"
  db:
    image: tomee
    ports:
      - "8050:8080"

services:
  wordpress:  # WordPress service
    image: wordpress:latest
    ports:
      - "8080:80"  # Map port 80 of the container to port 8080 of the host
    environment:
      WORDPRESS_DB_HOST: db:3306  # Database host
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db  # Ensures the db service starts first

  db:  # MySQL service
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: demo
      POSTGRES_PASSWORD: demo
      POSTGRES_DB: demo_db

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    depends_on:
      - redis
  redis:
    image: redis:alpine

docker-compose up -d
docker-compose down

(if multi) docker-compose up --scale <service name>=2 –d



Multi module Maven project:


1.Open eclipse-> file-> new -> other -> Maven -> MavenProject ->click on next.
2.Check the first option: Create a Simple project(skip archetype selection) and click on next.
3.Give groupID: KMIT, artifact ID: MultiModule, in the packaging tab select the drop down and opt for pom.
4.Give Name: Multimodule creation and Description: Sample multimodule with parent child hierarchy and click on next.
5.Now the Multimodule folder is created and displayed in the project explorer on the left. Right click on the folder and opt new -> mavenModule
6.Check the option Create a Simple project(skip archetype selection, give the artifactID: MultiModuleChild1 and click on next and finish.
7.Now you can see, the module1 we created is displayed in the project explorer with a separate pom.xml file.
8.Right click on the MultiModule folder in the project explorer and opt new -> mavenModule
9.Give the artifactID: MultiModuleChild2 and click on next.
10.In filters search for maven-archetype-webapp, select the version 1.1/1.5 and click on next.
11.Now click on finish, and confirm with a Y in the console.
12.Now you can watch the new child is been added to the parent project.



For Pipeline for web project:


1. Clone repo, tomcat 9.x run on server. make sure it's running locally.
2. FP Build : scm git, build steps clean install; pbs archive qrtifacts **/*, build test
2. FP test : scm none, build steps copy from another **/*; goals test, pbs archive qrtifacts **/*, build test
3. FP deploy : scm none, bs copy from another **/*; pbs deploy war **/*.war Webpath Tomcat 9.x remote


For script


1. create new item pipeline
2.triggers build periodically H * * * *
3.pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                echo "Cloning repository..."
                deleteDir()
                git url: 'https://github.com/NaveenCK-10/selab-internal.git', branch: 'main'
            }
        }

        stage('Clean') {
            steps {
                echo "Cleaning project..."
                bat "mvn clean"
            }
        }

        stage('Compile & Install') {
            steps {
                echo "Compiling and installing..."
                bat "mvn install"
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running test cases..."
                bat "mvn test"
            }
        }

        stage('Package Application') {
            steps {
                echo "Packaging application..."
                bat "mvn package"
            }
        }

        stage('Final Output') {
            steps {
                echo "Build Pipeline Completed Successfully!"
            }
        }
    }
}
4. use groovy sandbox


minikube


curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-windows-amd64.exe\n
move minikube-windows-amd64.exe C:\Windows\System32\minikube.exe\n
minikube start --driver=docker\n
minikube kubectl -- get pods -A\n
kubectl create deployment mynginx --image=nginx\n
kubectl expose deployment mynginx --type=NodePort --port=80 --target-port=80\n
kubectl scale deployment mynginx --replicas=4\n
kubectl get service myngnix\n
kubectl port-forward svc/mynginx 8081:80\n


nagios


docker pull jasonrivers/nagios:latest\n
docker run --name nagiosdemo -p 8888:80 jasonrivers/nagios:latest\n


ngrok


ngrok config add-authtoken <your_token>
ngrok http 8080
(Jenkins port)


Jenkins ci webhook


1. ngrok.exe http <Jenkins local host:8080> 
get https://wintry-gloryingly-nam.ngrok-free.dev /github-webhook/ (url+ add /github-webhook/)
2. GitHub -> webhoooks -> add link; application/json; just push event
3. check GitHub hook trigger for gitscm polling


Email setup:

1. Go to config system
2. Email notification section: SMTP Server - smtp.gmail.com; Use SMTP Auth - Enabled; User Name	- Gmail ID; Password - App Password; Use SSL - Enabled; SMTP Port - 465; Reply-To Address - same gmail id
3. Click test config and check email
4. Extended email notification section: smtp server, port, ssl is same; creds - email and app password; default content type - text/html; triggers - as needed
5. Add post-build action -> Editable Email Notification
6. recipient list : your email, content type : text/html, triggers : as needed


aws


1. create ec2 isntaance, new keypair, launch and connect to the instance with ubuntu os
ssh -I\n
sudo apt update\n
sudo apt-get install docker.io\n
sudo apt install git\n
Sudo apt install nano\n
git clone\n
Nano Dockerfile FROM nginx:alpine COPY . /usr/share/nginx/html FROM tomcat:9-jdk11 COPY target/*.war /usr/local/tomcat/webapps\n
sudo docker build -t mywebapp . \n
sudo docker run –d –p 80:80 mywebapp\n
