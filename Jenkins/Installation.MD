# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.

### Prerequisites
1. AWS `EC2 Instance` --> `RHEL` OS, `2Core` CPU and `4GB` RAM
   - With Internet Access
   - Security Group open Port `8080` for internet
2. Java 11.x

### Java Installation

https://github.com/futuretechdevops/Futuretech-DevOps-Demos/blob/master/Jenkins/Java_Installation.MD

## Install Jenkins
You can install jenkins using the rpm or by setting up the repo. We will setup the repo so that we can update it easily in future.

Get latest version of jenkins from https://pkg.jenkins.io/redhat-stable/

Or check the Jenkins official documentation for linux installation https://www.jenkins.io/doc/book/installing/linux/
```sh
yum -y install wget
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
yum upgrade
# Add required dependencies for the jenkins package
yum install jenkins
systemctl daemon-reload
```

### Start Jenkins
```sh
# Start jenkins service
systemctl start jenkins

# Setup Jenkins to start at boot,
systemctl enable jenkins
```

#### Accessing Jenkins
By default jenkins runs at port `8080`, You can access jenkins at
```sh
http://YOUR-SERVER-PUBLIC-IP:8080
```
#### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
  - Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
  - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  
 
	```sh
	Name : OpenJdk-11
	JAVA_HOME : /usr/lib/jvm/java-11-openjdk-11.0.17.0.8-2.0.1.el7_9.i386
	```
- Create another admin user id

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under Build section
	Execute shell : echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"

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
