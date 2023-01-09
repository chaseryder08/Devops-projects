# Vprofile Project Setup

<img src="images/base.jpg">

## OBJECTIVE: VM automation locally

create local set up 
* automated
* repeatable
* IAC 
  
TOOLS:
* need VM / hypervisor - Virtual box
* automation - use vagrant
* CLI - GITBASH
* IDE - VSCode

### ARCHITECTURE:
- NGINX
- TOMCAT
- RABBITMQ
- MEMCACHED
- MYSQL

### FLOW OF STACK
"collection of services working together to put togehter experience"

* user opens up browser enters IP address of load balancer
* NGINX - web application service / used to create load balancing experience > 
* when request comes, it routes request to TOMCAT service (APACHE TOMCAT - Java webapplication service)
 
- user login stored in MYSQL DB
- RABBIT MQ - message broker (aka queing agent)
- web application stored on apache tomcat 
- memcache - db caching
- mqsql stores request in memcached famous DB service

### AUTOMATION STACK
- VAGRANT - automate create VM machine from virtual box
- Virtual box
- BASH - set up services

<hr>

#### STEP 1: 
> vagrant file uses file to launch 5 Virtual machine
- NGINX
- TOMCAT
- RABBITMQ
- MEMCACHED
- DB machine

> need to install vagrant plugin before bringing up VM:
"vagrant plugin install vagrant-hostmanager"

<img src="images/VM_spinup.PNG">

#### STEP 2: Spin up Virtual Machines
> command - "Vagrant up" (Turns on all VM)
* spins up VM through vagarnt file
* takes a while to spin up all VM
* can see VM in Virtual box manager 

<img src="images/vm_spunup_list.PNG">

#### STEP3: Confirm VM up/pinging/check /etc/hosts file

1) I then SSH into web01 "vagrant ssh web01"
2) cat /etc/hosts - check etc/host file - updates
3) able to successfully ping app01
4) ssh into app01

#### STEP 4: Provisioning
1) Set up MySQL - login to db01 (MySQL server)
2) update all machines on 
3) save password variable for db - DATABASE='admin123' in /etc/profile file 
4) source /etc/profile (makes permanant
5) install epel-release repo 
6) yum install git mariadb-server
7) systemctl start/enable mariadb (ensure starts on boot)
8) mysql_secure_installation - set up mariadb / set password
9) mysql -u root -p (login)
10) clone sourcecode from repo for sql file in resources dir - 'git clone https://github.com/devopshydclub/vprofile-project.git'
11) set up database - mysql -u root -p "$DATABASE_PASS" -e "create database accounts"
12) add user in sql named 'admin' - admin can gain access to app01
13) login into db - execude db_backup.sql file to accounts db -- mysql -u root -p"$DATABASE_PASS" accounts < src/main/resources/db_backup.sql
14) mysql -u root -p"DATABASE_PASS" -e "FLUSH PRIVILEGES"
15) login in mysql/maria db - confirm have DB
16) show accounts - initializes db

### STEP 5: set up memcached server
1) login into md01
2) yum install epel- release & memcached
3) systemctl start memcached / enable
4) memcached -p 11211 -u 11111 -u memcached -d (allows to listen to port )
<img src="images/mc_port_open.PNG">
5) ss -tunlp | grep 11211 - confirm memcache port is open on 11211

### STEP 6: login into Rabbit MQ - set up server
1) login to rmq (rabbit MQ server) / yum update
2) wget install erlang & socat dependencies
3) install Rabbitmq server using curl shell script
4) script run, install rabbitmq server package / install and enable server
5) configuration setting change to RMQ -
<code> sudo sh -c 'echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config'</code>
6) create user - #sudo rabbitmqctl add_user test test
7) set user tag - sudo rabbitmqctl set_user_tags test administrator
8) bounce rabbitmq server - systemctl restart rabbitmq-server
9) Enabling the firewall and allowing port 25672 to access the rabbitmq permanently

### STEP 7 : TOMCAT SETUP
1) ssh into app01 server - Tomcat VM
2) update / set repositiory epel-release
3) install yum install java-1.8.0-openjdk
4) install git mavent wget
5) nav to /tmp folde - download tomcat package
6) extract tomcat package
7) create user for tomcat and home dir (copy all tomcat binaries) /usr/local/tomcat8
<img src="images/tomcat_copy.PNG">
8) change ownership to tomcat user > "chown tomcat.tomcat /usr/local/tomcat8 -R (everything owner by tomcat user)
9) Setup systemd for tomcat
> update file with content : 
<br>
<code>[Unit]
Description=Tomcat
After=network.target
[Service]
User=tomcat
WorkingDirectory=/usr/local/tomcat8
Environment=JRE_HOME=/usr/lib/jvm/jre
Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_HOME=/usr/local/tomcat8
Environment=CATALINE_BASE=/usr/local/tomcat8
ExecStart=/usr/local/tomcat8/bin/catalina.sh run
ExecStop=/usr/local/tomcat8/bin/shutdown.sh
SyslogIdentifier=tomcat-%i
[Install]
WantedBy=multi-user.target
* This runs this bash script upon boot up of tomcat*
</code>
10) systemctl daemon-reload (this saves any changes to service files)
11) systemctl enable tomcat

### STEP : CODEBUILD & DEPLOY (app01)
1) git clone -b local-setup https://github.com/devopshydclub/vprofile-project.git
2) vi src/main/resources/application.properties - *VERY IMPORTANT CONFIG FILE - changes for any username/pass for other servers
3) 
