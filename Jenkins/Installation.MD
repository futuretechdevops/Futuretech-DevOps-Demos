# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.

## Prerequisites
1. Create AWS `EC2 Instance` --> `RHEL` OS, `2Core` CPU and `4GB` RAM
   - With Internet Access
   - Security Group open Port `8080` for internet
2. Install Java 11.x
3. Install Git

## Steps
1. Create AWS `EC2 Instance`
2. Install Java 11.x
3. Install Git
4. Install Jenkins
5. Start Jenkins
6. Access Jenkins from UI
7. Setup Initial Configuration:  
   - Install default plugins.
   - set user name and password.
8. Configure `java` path on:
   - `Manage Jenkins` > `tools` > `JDK`
9. Create `Test` Jenkins Job. 

### 1. Create AWS `EC2 Instance`
```
Check `2-Day AWS Account Creation` documents
```

### 2. Install Java 11.x
https://github.com/futuretechdevops/Futuretech-DevOps-Demos/blob/master/Jenkins/Java_Installation.MD

### 3. Install Git
```
# Fedora
sudo yum install git

# Ubuntu
sudo apt-get install git
```

### 4. Install Jenkins
You can install jenkins using the rpm or by setting up the repo. We will setup the repo so that we can update it easily in future.
Get latest version of jenkins from :: https://pkg.jenkins.io/redhat-stable/
Or check the Jenkins official documentation for linux installation https://www.jenkins.io/doc/book/installing/linux/
```sh
sudo yum -y install wget
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade
yum install jenkins
sudo systemctl daemon-reload
```

### 5. Start Jenkins
```sh
sudo systemctl start jenkins    # --> start | stop | status | restart
or
sudo service jenkins start

# To start the Jenkins automatically after system reboot.
sudo systemctl enable jenkins
```

### 6. Access Jenkins from UI
By default jenkins runs at port `8080`, You can access jenkins at
```sh
http://<YOUR-SERVER-PUBLIC-IP>:8080
```

### 7. Setup Initial Configuration: 
- The default Username is `admin`
- Grab the default password 
  - Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Install suggested plugins`
- Set username and password
- Change admin password
  - `Admin` > `Configure` > `Password`

### 8. Configure `java` path on:
- Configure `java` path
  - `Manage Jenkins` > `Tools` > `JDK` 

	```sh
	Name : OpenJdk-11
	JAVA_HOME : /usr/lib/jvm/java-11-openjdk-11.0.17.0.8-2.0.1.el7_9.i386
	```

### 9. Create `Test` Jenkins Job. 
1. Go to `Dashboard`
2. Select `New Item`
3. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
4. Under `Build Steps` select > `Execute shell`
      ```
      echo "Welcome to Jenkins Demo"
      ```
5. Select `Save` 
6. Select `Build Now`
7. Check `Console Output` logs.



# Install Jenkins on Docker
### Dockerfile
```sh
FROM oraclelinux:7.9
LABEL maintainer="futuretechdevops@gmail.com"
USER root
RUN yum update -y && yum -y install java-11-openjdk.i686 wget unzip
RUN wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
RUN rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
RUN yum install jenkins -y
RUN chown -R jenkins:jenkins /var/lib/jenkins
ENV JENKINS_HOME=/var/lib/jenkins
ENV JENKINS_WEBROOT=%C/jenkins/war
ENV JAVA_OPTS=-Djava.awt.headless=true
ENV JENKINS_PORT=8080

ADD https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.6.0/apache-maven-3.6.0-bin.tar.gz /opt
RUN tar -xzf /opt/apache-maven-3.6.0-bin.tar.gz -C /opt
RUN rm -f /opt/apache-maven-3.6.0-bin.tar.gz

EXPOSE 8080 8090
ENTRYPOINT "/usr/bin/jenkins"
```
