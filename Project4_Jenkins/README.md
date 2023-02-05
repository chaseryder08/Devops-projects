# Project 4 : Jenkins Pipeline as Code

Concepts:
* pipeline - all jobs getting executed - main block executed by henkins
* node/agent - which agent getting exectued
* stage > steps in stage / get pull, upload artifcat, any step want executed

===
## STEPS:

1) create SG

2) create ec2 ubuntu jenkins
3) tall jenkins w/ user data:

<code>
#!/bin/bash
sudo apt update
sudo apt install openjdk-11-jdk -y
sudo apt install maven -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
</code>

===

http://54.208.180.206:8080/

## SETUP JENKINS:

1) unlock jenkins
2) go to Jenkins home dir - cd /var/lib/jenkikns
3) get password - d6d33c20e5ca4383b5393ce28904e548
4) add node.js plugin
5) user: admin pass: admin - create admin

> jenkins URL: http://54.226.100.231:8080/

===

## Create JOB
need to make sure we have maven, and jdk8 

1) Manage Jenkins > Global Tools config > add JDK
2) Install JDK in cli, ssh into Jenkins>
- sudo apt install openjdk-8-jdk
3) copy jdk8 and put in java home on jenkins "/usr/lib/jvm/java-1.8.0-openjdk-amd64" and add Maven

![alt text](img)

