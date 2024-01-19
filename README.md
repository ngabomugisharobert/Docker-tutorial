# Tutorial - Docker Containers

## Setup

### Lab VM
Log into your Azure Portal and start your Lab VM

In Azure cloud shell, open port 8080 on the lab VM
  
    az vm open-port --resource-group lab1 --name labvm --port 8080

Log into the Lab VM via ssh
  
    ssh -i <path-to-private-key> username@ip-address 
   
### Docker Setup & Maven setup
Install Docker on your Lab VM

    sudo apt install docker.io

### Install Java 17

  sudo apt install openjdk-17-jdk openjdk-17-jre
  
### Install Maven
Maven is used to build and package a java application. Run the command below to install maven.

    sudo apt install maven
    
    
## Setup PetClinic Java Spring Boot Application
    
    # create lab directory
    cd ~
    mkdir labs
    cd labs
    mkdir containers
    cd containers
    
    # Clone petclinic spring boot application
    git clone https://github.com/spring-projects/spring-petclinic.git
    
    cd spring-petclinic
 
### Run the web app
    # Package the application
    mvn package
    
    # Run the executable
    java -jar target/spring-petclinic-3.1.0-SNAPSHOT.jar

## Dockerize the executable

### Create the Docker File
    nano Dockerfile
    
### Paste these into the Dockerfile
    
    FROM ubuntu:20.04

    COPY ./target/spring-petclinic-3.1.0-SNAPSHOT.jar .

    EXPOSE 8080

    RUN apt-get update && \
        apt-get install -y openjdk-17-jdk

    CMD ["java", "-jar", "spring-petclinic-3.1.0-SNAPSHOT.jar"]
    
 ### Save and Exit the Editor
 CTRL + O
 CTRL + X
 
 ## Build the docker image
    
    sudo docker build -t petclinic-app .
    
 ## Run the container
    
    sudo docker run -d -p 8080:8080 petclinic-app
 
 