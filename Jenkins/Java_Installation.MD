### Install Java
We will be using open java for our demo, Get latest version from http://openjdk.java.net/install/
```sh
yum update
yum install java-11*
#yum -y install java-11-openjdk.x86_64q
```

### Check Java Version
Lets install java and set the java home
```sh
java -version
```

_The output should be something like this,_
```
[root@~]# java -version
openjdk version "11.0.17" 2022-10-18 LTS
OpenJDK Runtime Environment (Red_Hat-11.0.17.0.8-2.el9_0) (build 11.0.17+8-LTS)
OpenJDK 64-Bit Server VM ((Red_Hat-11.0.17.0.8-2.el9_0) (build 11.0.17+8-LTS, mixed mode, sharing)
```

### Optional JAVA HOME configuration
##### Find java binaryfile location
```sh
which java
ls -lrth <above-command-output-filename>
(OR)
readlink -f `which java`
```
```sh
##### Set java_home and path
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.19.0.7-4.el9.x86_64
PATH=$PATH:$JAVA_HOME
# To set it permanently update on your home directory .bash_profile file
source ~/.bash_profile
```
