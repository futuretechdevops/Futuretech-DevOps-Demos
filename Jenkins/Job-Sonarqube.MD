#  Jenkins Sonarqube job configuration

#### Prerequisites
1. Jenkins server
2. Sonar Server With DB installation (EC2 node).
3. Sonar Scanner Installation (Jenkins Server).
4. Sonar Scanner plugins installed (Jenkins UI).
5. Sonar Scanner path configuration on 'Global Tool Configuration' (Jenkins UI)
6. Generate SonarQube Server authentication token (SonarQube Server UI)
7. Sonar Server configuration on 'Configure System' (Jenkins UI)
8. Creat `Sonar-test` new Project (SonarQube Server UI)
9. In Jenkins Create Sonar-Qube Jobs (Jenkins UI)

#### 5. Sonar Scanner path configuration on 'Global Tool Configuration' (Jenkins UI)
`Manage Jenkins` > `Global Tool Configuration` > `SonarQube Scanner` > `SonarQube Scanner installations` > `Add SonarQube Scanner`
   - Name               : **sonar-scanner-4.7**
   - SONAR_RUNNER_HOME  : **/opt/sonar_scanner**

#### 6. Generate SonarQube Server authentication token (SonarQube Server UI)
Server authentication token To Get Authentication code follow below steps. Login to SonarQube server as a admin `My Account` > `Security` > `Generate Token`

#### 7. Sonar Server configuration on 'Configure System' (Jenkins UI)
`Manage Jenkins` > `Configure System` > `SonarQube servers` > `Add SonarQube`
   - Name               : **SonarQube-Server-CE-9.8**
   - Server URL         : **http://172.31.0.224:9000/sonar**
   - Server authentication token  :  `Add` > `Jenkins`
          - Kind   : **Secret Text**
          - Secret : **Copy From above 6 step generated Token**
          - ID     : **SonarQube-Token*

#### 8. Creat `Sonar-test` new Project (SonarQube Server UI)
 Login to SonarQube server as a admin 
 `Administration` > `Project` > `Management` > `Create Project`
 
     - Name : sonar-test-1
     - Key  : sonar-test-1

#### 9. In Jenkins Create Sonar-Qube-test Jobs (Jenkins UI)
1. Create - `new item`
2. Enter the job name : **Sonar-Qube-Test**
   - Chose `Freestyle` project
3. Under `Source Code Management` > `GIT`
   - add the Repository URL : **https://github.com/futuretechdevops/sonar-scanning-examples.git**
   - add the Branch name : */master
4. Under `Build Steps` > `Execute shell` 
   ```
cd sonarqube-scanner-maven/maven-basic

/opt/maven/apache-maven-3.6.0/bin/mvn clean verify

/opt/sonar_scanner/bin/sonar-scanner \
  -D sonar.projectName='sonar-test-1' \
  -D sonar.projectKey=sonar-test-1 \
  -D sonar.exclusions=vendor/**,resources/**,**/*.java \
  -D sonar.login=8d5adc71e906df4b902148c87182e7c9cbfc9698 \
  -D sonar.projectVersion='1.0-SNAPSHOT' \
  -D sonar.sources="${WORKSPACE}/sonarqube-scanner-maven/maven-basic" \
  -X
     ```
 Save and Run The Build and Validaat the Status on Sonar UI.
