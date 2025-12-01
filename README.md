hello
















































docker redis
1. docker pull redis
2. docker run --name my-redis -d redis
3. docker ps
4. docker exec -it my-redis redis-cli
5. 127.0.0.1:6379> SET name "Alice"
OK
127.0.0.1:6379> GET name
"Alice"
127.0.0.1:6379>exit // to come out
6. docker stop <container_id>
7. docker rm my-redis
8. docker rmi redis


Dockerfile
1. docker build -t redisnew  .
What it does:
• This creates (builds) a Docker image using the recipe (Dockerfile) in the current
folder (.).
• -t redisnew: Gives the image a name/tag ("redisnew"), so you can find it easily.
2. docker run --name myredisnew -d redisnew
What it does:
• Starts a new container (mini computer) from the redisnew image.
• --name myredisnew: Names the container "myredisnew" so it’s easy to identify.
• -d: Runs the container in the background.
3. docker ps
What it does:
• Shows a list of containers that are running right now.
4. docker stop myredisnew
What it does:
• Stops the container named "myredisnew" (like turning off a computer).
5. docker login
What it does:
• Logs you into your Docker Hub account, so you can upload images.
6. docker ps -a
What it does:
• Shows a list of all containers, including stopped ones.
7. docker commit 0e993d2009a1 budarajumadhurika/redis1 
// Note 0e993d2009a1 is container id of myredisnew
What it does:
• Takes a snapshot (saves changes) of the container with ID 0e993d2009a1 and creates a
new image called budarajumadhurika/redis1.
8. docker images
What it does:
• Lists all images saved on your system. // will show image of budarajumadhurika/redis1
9. docker push budarajumadhurika/redis1
What it does:
• Uploads the image budarajumadhurika/redis1 to Docker Hub, so others can download
it.
10. docker rm 0e993d2009a1  // container id of myredisnew
What it does:
• Deletes the container with ID 0e993d2009a1 of myredisnew.
11. docker rmi budarajumadhurika/redis1
What it does:
• Deletes the image budarajumadhurika/redis1 from your system.
12. docker ps -a
What it does:
Shows all containers again to confirm changes.
13. docker logout
What it does:
• Logs you out of Docker Hub.
14. docker pull budarajumadhurika/redis1
What it does:
• Downloads the image budarajumadhurika/redis1 from Docker Hub.
15. docker run --name myredis -d budarajumadhurika/redis1
What it does:
• Starts a new container using the image budarajumadhurika/redis1.
16. docker exec -it myredis redis-cli
What it does:
• Opens the Redis command-line interface (like a terminal) inside the running container
myredis.
17. SET name "Abcdef"
What it does:
• Saves a key-value pair in Redis (key = name, value = Abcdef).
18. GET name
What it does:
• Retrieves the value of the key name from Redis (it will return "Abcdef").
19. exit
What it does:
• Exits the Redis CLI.
20. docker ps -a
What it does:
• Shows all containers again to check their status.
21. docker stop myredis
What it does:
• Stops the container myredis.
22. docker rm 50a6e4a9c326
What it does:
• Deletes the container with ID 50a6e4a9c326.
23. docker images
What it does:
• Lists all images again to confirm which ones remain.
24. docker rmi budarajumadhurika/redis1
What it does:
• Deletes the image budarajumadhurika/redis1 again.
Step 3: Remove Login Credentials (Optional)
If you no longer need to be logged in, you can log out:
docker logout
What It Does:
• Logs you out from Docker Hub and removes your stored credentials.





docker-compose
1. docker-compose.yaml
2. docker-compose up -d
3. docker-compose down
4. docker-compose up --scale <service name>=2 -d


I.	Create a new folder compose-lab
Inside it, create a file docker-compose.yml with the following content:

version: "3.9"
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

II. Run the setup:

docker compose up -d

III. Open your browser and visit: http://localhost:8080.

IV. Expected Output:
Nginx welcome page is displayed.
db container runs in the background.

2.Write and interpret docker-compose.yml files
Task:
I.	Modify docker-compose.yml to add a Redis cache:

  redis:
    image: redis:alpine

II. Add a depends_on so web waits for Redis:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    depends_on:
      - redis

III. Restart the setup:
docker compose up -d
docker compose ps

IV. Expected Output:

Three services (web, db, redis) are listed as running.

3.Deploy across different machines
Task:
I.	Zip your compose-lab folder.

Transfer it to another machine with Docker Compose installed.

II. Run:
docker compose up -d

Check that Nginx and Postgres work there as well.

III. Expected Output:
The same services run on the new machine without changes.

4.Networking and persistent storage
Task:
I.	Update your docker-compose.yml to add a custom network and volume:

networks:
  app-net:

volumes:
  db-data:

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    networks:
      - app-net
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: demo
      POSTGRES_PASSWORD: demo
      POSTGRES_DB: demo_db
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - app-net

II. Run:
docker compose up -d
III. Insert some data into Postgres (optional with psql).
IV. Remove containers:
docker compose down
V. Start again:
docker compose up -d
VI. Expected Output:
Database data persists across restarts.
Services communicate via the app-net network using service names.





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
