hello



























































For Pipeline for web project:

1. Clone repoo, tomcat 9.x run on server. make sure it's running locally.
2. FP Build : scm git, build steps clean install; pbs archive qrtifacts **/*, build test
2. FP test : scm none, build steps copy from another **/*; goals test, pbs archive qrtifacts **/*, build test
3. FP deploy : scm none, bs copy from another **/*; pbs deploy war **/*.war Webpath Toomcat 9.x remote

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

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-windows-amd64.exe
move minikube-windows-amd64.exe C:\Windows\System32\minikube.exe
minikube start --driver=docker
minikube kubectl -- get pods -A
kubectl create deployment mynginx --image=nginx
kubectl expose deployment mynginx --type=NodePort --port=80 --target-port=80
kubectl scale deployment mynginx --replicas=4
kubectl get service myngnix
kubectl port-forward svc/mynginx 8081:80


nagios

docker pull jasonrivers/nagios:latest
docker run --name nagiosdemo -p 8888:80 jasonrivers/nagios:latest


Jenkins ci webhook

1. ngrok.exe http <Jenkins local host:8080> get https://wintry-gloryingly-nam.ngrok-free.dev /github-webhook/
2. GitHub -> webhoooks -> add link; application/json; just push event
3. check GitHub hook trigger for gitscm polling

aws

1. create ec2 isntaance, new keypair, launch and connect to the instance with ubuntu os
ssh -I
sudo apt update
sudo apt-get install docker.io
sudo apt install git
Sudo apt install nano
git clone
Nano Dockerfile FROM nginx:alpine COPY . /usr/share/nginx/html FROM tomcat:9-jdk11 COPY target/*.war /usr/local/tomcat/webapps
sudo docker build -t mywebapp .
sudo docker run –d –p 80:80 mywebapp
