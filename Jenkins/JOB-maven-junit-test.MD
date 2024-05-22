#  Jenkins maven-junit-test job configuration

#### Prerequisites
1. Jenkins server
2. Maven installed on Jenkins node
3. Maven plugins installed on Jenkins UI.

#### maven-junit-test Jobs
1. Create - `new item`
1. Enter the job name : **maven-junit-test**
   - Chose `Freestyle` project
1. Under `Source Code Management` > `GIT`
   - add the Repository URL : **https://github.com/mkyong/maven-examples.git**
   - add the Branch name : */master
1. Under `Build Steps` > `Invoke top-level Maven targets`
   - Select : **Maven Version**
   - Goals : **clean test**
   - POM : **maven-unit-test**

1. Publish JUnit Reports
1. Under `Post-build Actions` > `Publish JUnit test result report`
   - Test report XMLs :  **maven-unit-test/target/surefire-reports/*.xml**
