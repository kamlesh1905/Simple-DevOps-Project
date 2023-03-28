# Installing Docker on Amazon Linux server

### Pre-requisites
1. Amazon Linux EC2 Instance

## Installation Steps

1. Install docker and start docker services
   ```sh 
   yum install docker -y
   docker --version 
   
   # start docker services
   service docker start
   service docker status
   ```
1. Create a user called dockeradmin
   ```sh
   useradd dockeradmin
   passwd dockeradmin
   ```
1. add a user to docker group to manage docker 
   ```
   usermod -aG docker dockeradmin
   ```
   
1. Allow ec2-user to accept password based auth 
   ```
   vi /etc/ssh/sshd_config
   
   ##enable password based authentication
   
   service sshd reload
   ```
1. Create docker folder under /opt and provide dockeradmin rights to folder recursively.This folder will help to copy artifact and make dockerfile to create image and container
   
   chown -R dockeradmin:dockeradmin docker 
   
   
## Pull Docker Image

1.docker pull tomcat

docker images
`````````````
cmd - docker images

REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
tomcat       latest    7a91e6f458bb   6 days ago   475MB

launch container
````````````````
docker run -d --name mycontainer -p 8080:8080 tomcat

CONTAINER ID   IMAGE     COMMAND             CREATED          STATUS          PORTS                                       NAMES
752bd51a929d   tomcat    "catalina.sh run"   14 minutes ago   Up 14 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   mycontainer

login to this docker container
``````````````````````````````
docker exec -it mycontainer /bin/bash

login to container path --->root@752bd51a929d:/usr/local/tomcat#

cd /webapps.dist

cp -R * ../webapps/

Now check tomcat server working or not

Ipaddress:8080 

Install Vi editor in docker container
`````````````````````````````````````
apt-get update
apt-get install vim

### Access tomcat application from browser on port 8080

(http://<Public_IP>:8080

now application is accessible on port 8080. but tomcat application doesnt allow to login from browser. changing a default parameter in context.xml does address this issue

#search for context.xml
find / -name context.xml
above command gives 3 context.xml files. comment () Value ClassName field on files which are under webapp directory. After that restart tomcat services to effect these changes. At the time of writing this lecture below 2 files are updated.

vi /usr/local/tomcat/webapps/host-manager/META-INF/context.xml
vi /usr/local/tomcat/webapps/manager/META-INF/context.xml

ln -s /usr/local/tomcat/bin/startup.sh /usr/local/bin/tomcatup
ln -s /usr/local/tomcat/bin/shutdown.sh /usr/local/bin/tomcatdown

tomcatup

# Restart tomcat services

tomcatdown  
tomcatup

Update users information in the tomcat-users.xml file goto tomcat home directory and Add below users to conf/tomcat-users.xml file
 <role rolename="manager-gui"/>
 <role rolename="manager-script"/>
 <role rolename="manager-jmx"/>
 <role rolename="manager-status"/>
 <user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
 <user username="deployer" password="deployer" roles="manager-script"/>
 <user username="tomcat" password="s3cret" roles="manager-gui"/>
Restart serivce and try to login to tomcat application from the browser. This time it should be Successful)

   
### Validation test
1. Create a tomcat docker container by pulling a docker image from the public docker registry
   ```sh
   docker run -d --name test-tomcat-server -p 8090:8080 tomcat:latest
   ```

## Docker Installation on CentOS server
##### Referance URL : https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository
### Pre-requisites

Please follow below steps to install docker CE on CentoOS server instance. For RedHat only Docker EE available 

1. Install the required packages.

   ```sh 
   sudo yum install -y yum-utils \
   device-mapper-persistent-data \
   lvm2
   ```
  
1. Use the following command to set up the stable repository.
 
   ```sh 
   sudo yum-config-manager \
   --add-repo \
   https://download.docker.com/linux/centos/docker-ce.repo
   ```

### INSTALLING DOCKER CE

1. Install the latest version of Docker CE.
   ```sh 
   sudo yum install docker-ce
   ```

   Note: If prompted to accept the GPG key, verify that the fingerprint matches 
060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, and if so, accept it.

1. Start Docker.
   ```sh 
   sudo systemctl start docker
   ```

1. Verify that docker is installed correctly by running the hello-world image.
   ```sh
   sudo docker run hello-world
   ```
