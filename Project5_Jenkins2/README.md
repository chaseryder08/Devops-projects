# Project 5: Jenkins CI and Tools

![img](img/jenkins_main.PNG)

## Problems
- Agile SDLC
- regular code changes everyday, code needs to be tested regularly
- devopers need to fix bugs as they come / delays process
- manual, time consuming

## FIX
- build and test for every commit
- automated build and release process
- if build fail, devs will get notification - first fix code

## SOLUTION
### CONTINOUS INTEGRATION!
- many tools paired together to automate the process

## PRO:
- short time to repair
- fault isolation
- agile
- fast turn around time

## CREATE CI PIPELINE
## TOOLS: and sidekicks
- Jenkins - CI server
- GIT - Version control system
- Maven - build tool
- Checkstyle - code analysis tool
- Slack - notification / emails too
- Nexus - artifact software repository
- SonarQube - code analysis server
- AWS EC2 - computer resource / Jenkinks, nexus, solarqube

### PROCESS: automated, breaks set at many points :
1) Developers commit code to GIT *(Jenkins triggers with git commit)*
2) Jenkins detect GIT push : fetch newest code 
3) Jenkins next run UNIT TESTS - if fails notification sent / IF pass, code analysis will be done by CHECKSTYLE/SONARSCANNER*...
4) Generate report, will store on SONARQUBE - Quality gates - if pass quality gates
6) BUILD artifact with MAVEN (java)> packaged > artifact will be uploaded > notification will be sent
7) Code will need maven dependencies (stores in NEXUS repositories)
8) Artifact packages and stored in artifact repository (Nexus)

### STEPS:
1) Login AWS
2) Create keypair
3) create Jenkins SG (Jenkins, Nexus, Sonarqube)
4) Create ec2 for Jenkins, Nexus, SonarQube
5) Post installation
- jenkins setup and plugins
- nexus setup and repo setup
- sonarqube login test
6) Create GIT repo
7) Build job w/ nexus installation
8) Create Github webhook - set up automate Jenkins job trigger w/ github
9) Sonarqube server integrateion Stage

# STEP 1: Key pair and Security group
1) Create keypair
2) Create Jenkins security group
- open port 22 (ssh) 
- open port 8080 from all ipv4 & ipv6
- open port 8080 from Sonar-SG (need to add to let sonar connect to Jenkins)
3) Create Nexus security group
   - open port 22, 8081 from my IP, and 8081 from Jenkins (so jenkins can upload artifact to nexus server)
4) Create Sonar -SG
- open port 22, and port 80 from Jenkins SG (Jenkins upload test result on port 80 )