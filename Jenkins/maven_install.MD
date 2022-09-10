#  Install & configure Maven build tool on Jenkins
Maven is a code build tool which used to convert your code to an artifact. this is a widely used plugin to build in continuous integration


#### Prerequisites
1. Jenkins server

#### Install Maven on Jenkins
1. Download maven packages https://maven.apache.org/download.cgi onto Jenkins server. In this case, I am using /opt/maven as my installation directory
 - Link : https://maven.apache.org/download.cgi
    ```sh
     # Creating maven directory under /opt
     mkdir /opt/maven
     cd /opt/maven
     # downloading maven version 3.6.0
     wget https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz
     tar -xvzf apache-maven-3.6.1-bin.tar.gz
     
     OR 
     
     cd /opt
     # downloading maven version 3.6.0
     wget https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz
     tar -xvzf apache-maven-3.6.1-bin.tar.gz
     rm -f apache-maven-3.6.1-bin.tar.gz
     mv apache-maven-3.6.1-bin.tar.gz maven
     
     ```
	
1. Setup M2_HOME and M2 and JAVA_HOME paths in .bash_profile of the user and add these to the path variable
   ```sh
   vi ~/.bash_profile
   M2_HOME=/opt/maven/apache-maven-3.6.1
   M2=$M2_HOME/bin
   PATH=<Existing_PATH>:$M2_HOME:$M2
   ```
   or 
   
   vi ~/.bash_profile
   M2_HOME=/opt/maven
   M2=/opt/maven/bin
   JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.13.0.8-1.amzn2.0.3.x86_64
   PATH=<Existing_PATH>:$M2_HOME:$M2:$JAVA_HOME
   
   Command to find java path
   `````````````````````````
   find / -name java-11*
   
   source .bash_profile
   echo $PATH
   ```
   
#### Checkpoint 
1. logoff and login to check maven version
  
    ```sh
    mvn --version
    ````
So far we have completed the installation of maven software to support maven plugin on the jenkins console. Let's jump onto Jenkins to complete the remaining steps. 

### Setup maven on Jenkins console
1. Install maven plugin without restart  
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Invoker`
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Integration`

2. Configure maven path
  - `Manage Jenkins` > `Global Tool Configuration` > `Maven`

