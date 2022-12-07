#  Jenkins maven-junit-test configuration

#### Prerequisites
1. Jenkins server
2. Maven installed on Jenkins node
3. Maven plugins installed on Jenkins UI.

### maven-junit-test Jobs
1. Create - `new item`
1. Enter the job name – `maven-junit-test`
   - Chose `Freestyle` project
1. Under `Source Code Management` > `GIT`
   - add the Repository URL : https://github.com/mkyong/maven-examples.git
   - add the Branches to build : */master
1. Under `Build Steps` > `Invoke top-level Maven targets`
   - Select `Maven Version`
   - `Goals` : clean test
