#  Jenkins Nexus-Push job configuration

#### Prerequisites
1. Jenkins server
2. Maven installed on Jenkins node
3. Maven plugins installed on Jenkins UI.
4. Nexus Server installtion
5. **Nexus Artifact Uploader** plugin installation 

#### In Nexus UI Create the new repository
`Settings` > `Repositories` > `create repository` > `maven2 (hosted)`
   - Name            : **my-maven**
   - Version policy  : **Snapshot**
   - Blob Store      : **Default**
   - `Create repository`

#### Nexus-Push Jobs
1. Create - `new item`
1. Enter the job name : **Nexus-Push**
   - Chose `Freestyle` project
1. Under `Source Code Management` > `GIT`
   - add the Repository URL : **https://github.com/futuretechdevops/mavenrepo.git**
   - add the Branch name : */master
1. Under `Build Steps` > `Invoke top-level Maven targets`
   - Select : **Maven Version**
   - Goals : **clean compile package**
1. Under `Build Steps` > `Nexus artifact uploader`
   - Nexus Version : **NEXUS3**
   - Protocol      : **HTTP**
   - Nexus URL     : **54.227.152.232:8081/repository/my-maven**
   - Credentials   : **nexus-credentials**
   - GroupId       : **com.jdevs**
   - Version       : **2.5-SNAPSHOT**
   - Repository    : **my-maven**
   - ArtifactId    : **my-maven**
   - Type          : **war**
   - File          : **target/studentapp-2.5-SNAPSHOT.war**
