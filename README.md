# Overview
A Jenkins Project to automatically build a program if the code is modified on Github. Here, the program is simply a HelloWorld program but it can be replaced with any real time functional entry point Python script. We then use the Fire library to convert the script into CLIs. The trigger is based on PollSCM and the stages after trigger include Build, Test and Deliver. It is recommended to use BlueOcean to visualize and track these stages.

# Workflow
We first set up a master server and then the Docker cloud agent (We could also have used Kubernetes, AWS etc). We can then either set up a Freestyle Project or a Declarative Pipeline using Declarative Language Groovy.

## Installation
Build the Jenkins BlueOcean Docker Image
```
docker build -t myjenkins-blueocean:2.414.2
```

## Create the network 'jenkins'
```
docker network create jenkins
```

## Run the Container
### MacOS / Linux
```
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.414.2
```

### Windows
```
docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.414.2
```

## Get the Password
```
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

## Connect to the Jenkins
```
https://localhost:8080/
```
## Using my Jenkins Python Agent
```
docker pull devopsjourney1/myjenkinsagents:python
```
