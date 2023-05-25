## CI/CD Hello World using IAC

# Overview
This project is based on creation of two AWS EC2 instances and installation of Jenkins and Tomcat on both instances using Terraform (IAC) and Ansible (Config), Afterwards a basic Jenkins pipeline is created to link to SCM (GitHub) for deployment onto a Tomcat Server

# Steps

Create Two AWS EC2 instances and install Jenkins on 1 EC2 using Terraform
Create S3 bucket for storing installation file using Terraform
Tomcat installation on 1 EC2 using Ansible playbook
Jenkins Configuration & plugins
Tomcat server setup
Jenkins Pipeline + webhook & Jacoco Setup
Code deployment using GitHub

# Tomcat Server Setup

# Check point :

Access tomcat application from browser on port 8080

(http://your_public_dns_name:8080)
1. Tomcat application will not allow login from browser, therefore, changing the default parameter in context.xml will address this issue
       #search for context.xml
       find / -name context.xml
 
2. The above command gives 3 context.xml files, only the two below files which are under webapps directory needs updating, After that restart tomcat services to effect these changes

  /opt/tomcat/apache-tomcat-9.0.52/webapps/host-manager/META-INF/context.xml
  /opt/tomcat/apache-tomcat-9.0.52/webapps/manager/META-INF/context.xml
  
  # Restart tomcat services
   sudo systemctl restart tomcat
   
3.Update users information in the tomcat-users.xml file, goto tomcat home directory and Add below users to conf/tomcat-users.xml file

     <role rolename="manager-gui"/>
     <role rolename="manager-script"/>
     <role rolename="manager-jmx"/>
     <role rolename="manager-status"/>
     <user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
     <user username="deployer" password="deployer" roles="manager-script"/>
     <user username="tomcat" password="arinze" roles="manager-gui"/>
 
 
 
 # Restart tomcat services
   sudo systemctl restart tomcat
   
4. Restart serivce and try to login to tomcat application from the browser. This time it should be Successful


# Jenkins Plugins & Pipeline Setup

# Check point :

1. install deploy to container, Maven Integration and Jacoco plugins under Jenkins --> Manage Jenkins --> Manage plug-ins

2. Click on Available, type Deploy to container, select it. enter Jacoco, select it. Click on Install without restart.

# steps to automate WebApp project in Jenkins:
 
1. Login to Jenkins. Click on New item.
2. Enter an item name --> select Maven project.
3. Under source code mgmt, click git. GitHub URL https://github.com/anez247/Hello-world.git, leave credentials blank
4. Under branches to build, enter main
5. Under Build Triggers, select "GitHub hook trigger for GITScm polling"
6. Under Build, Enter "clean install" in Goals and options
7. Under Post-build actions drop down menu, select Record JaCoCo coverage report and Deploy war/ear to a container
8. For WAR/EAR files enter **/*.war
9. Select Tomcat 8 under container drop down menu
10.Enter Tomcat instance URL:8080
11.Click on add credentials, enter deployer as user name and deployer as password.
12.Click Apply, click Save
13.Click on build now.

Use Tomcat server endpoint URL http://ec2-18-130-35-46.eu-west-2.compute.amazonaws.com:8080/webapp/ to access the page
