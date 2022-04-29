# Jenkins_SampleFirstProject


How to run jenkins after installation ??
2 ways to Login into Jenkins - 1. Local Machine 2. AWS Cloud
- Go to jenkins.war folder location and run "java -jar jenkins.war" and go to the URL -> http://localhost:8080/
- Go to location of AWS pem instance key pair - `ssh -i Jenkins-key-Ohio.pem ubuntu@3.22.187.170` and to install anything, become root user by `sudo -i`
  Note 1: While using AWS, the login page is : public_ipv4_address:8080 -> ex: http://18.188.10.19:8080/ 
  Note 2: Dont worry about old projects, just login with new ipv4 address for same Ec2 instance, it will automatically reflect in the new one!

If it shows Unsupported Java version, Install it to Java 11 jdk version -> https://www.oracle.com/in/java/technologies/javase-jdk11-downloads.html
Note: Java 17 is not supported 

# simple and best source for Jenkins
If any doubts, go to https://www.javatpoint.com/jenkins for more info

4/7 : 

- Email Set up automated configuration - complete
- To know about the Environment variables in Jenkins : https://wiki.jenkins.io/display/JENKINS/Building+a+software+project#Buildingasoftwareproject-belowJenkinsSetEnvironmentVariables

# Maven build:
- Learnt about Maven build : It is an XML file that contains information about the project and configuration details used by Maven to build the project. It contains default values for most projects. ( Ex: pom.xml in eclipse which contains all config files)
- What is Maven build process?
There are three built-in build lifecycles: default, clean and site. The default lifecycle handles your project deployment, the clean lifecycle handles project cleaning, while the site lifecycle handles the creation of your project's web site.
- How to run Maven build using Jenkins : 2 ways : 
    1) run using windows batch command shell and give commands ( cd location and then mvn clean test) - depends on what we are doing - here cleaning it and testing the selenium test cases as per this git project (ref : https://github.com/Mukesh-50/mavenjenkins )
    2) Using Invoke top-level Maven projects 
        - Firstly, set global config maven settings and give maven path 
        - give what commands we need to run in goals section ( here, mvn clean test ) 
        - Imp : Give pom.xml file location (Root pom) so that it can read it directly by invoking it
   
   
4/13 
# Create Jenkins Job With Parameter

There will be so many parameters just like a google form 

- Dropdown, Boolean, File upload, Password credentials field and other
- https://www.baeldung.com/ops/jenkins-parameterized-builds for more info 
    
    
# Automatically Trigger Jenkins Jobs Using Github WebHook

Since Jenkins is CI/CD tool, We need to trigger the jobs automatically - Github Webhook is one of the way! We need to give authentication first and
we need to give github project link in jenkins inorder to trigger. But before that, we need to configure git.exe in global configuration in Jenkins.

If there is any change (Push/pull - any), It should trigger the Job

What is Web Hook? 

    We’ll send a POST request to the URL below with details of any subscribed events. You can also specify which data format you’d like to receive (JSON, x-www-form-urlencoded, etc). 

How to create Web hook : 
- Go to your project you want to trigger the alerts and select create webhook
- Provide Payload URL - here it could be either cloud or ngrok(public address of localhost) But It cant be the local host!
- If Jenkins is running on localhost, then create ngrok free URL: 

        - What is ngrok? - ngrok is cross-platform application that enables developers to expose a local dev server to internet cloud.
        - Commands : 
            To get the version -> ngrok --version
            To Run the ngrok -> ngrok.exe http 8080
- Payload URL would look like ->  http://5737-24-114-82-183.ngrok.io/github-webhook/ -> Note we need to add "/github-webhook/" to ngrok url
- Make sure you give HTTPS URL itself and not SSL
            
Steps : 
- In Jenkins/Configure - Firstly give  github link here
- select `GitHub hook trigger for GITScm polling` option
    
   
# Run Jenkins Job Periodically

- Read cron jobs and its format to execute it - just use any shell batch script and write cron format command to execute it periodically
- To check if the output is accurate, there is free cron tab calculator -> https://crontab.guru/


# Create Upstream and Downstream Jobs ( IMP )

Upstream - Jobs that needs to be run first 
Downstream - Jobs that need to be run at last

Ex : Job_1 -> Job_2 -> Job_3 
It is same like in real Environment: Build -> Test -> Deploy

- "Upstream_Downstream_Job_1" is Job 1
- "Upstream_Downstream_Job_2" is Job 2 
- "Upstream_Downstream_Job_3" is Job 3 

Here priority is 1>2>3

So How to create this:
- Select `Post-build actions` and then select `build other projects`
- Provide the project that needs to be run next (here proj 2)
- Likewise, Go to Proj 2 configure and do same and provide proj 3 in post build actions
- Now, Just execute the proj 1 and see the flow happening in the console ouput from 1 to 2 to 3

![image](https://user-images.githubusercontent.com/35003840/163107982-462e8e7b-c5e2-4fb2-86ce-a2ec3909f38a.png)


#### Imp Plugins   
- Parameterized Trigger and Build Pipeline View

- ZentimestampVersion - allows the customization of the date and time pattern for the Jenkins BUILD_TIMESTAMP variable


# Build Pipeline view :

- It helps in creating the pipeline flow and we just need to give the initial job to run
- We can also use `run` command in the view as shown in below figure

    ![image](https://user-images.githubusercontent.com/35003840/163109428-387895c4-f51b-4c91-9e40-51044e386967.png)

    
# What Is Jenkins Pipeline | Jenkins Pipeline Setup Example With Github | Jenkinsfile For Maven Build 

## Jenkins Pipeline Setup : 

Before starting, there is `Jenkinsfile` where we write the code about the stages and its syntax 
Go through this link for getting more info https://www.jenkins.io/doc/book/pipeline/jenkinsfile/
- Create a new project with `Pipeline` as project type instead of Freestyle proj
- In the Pipeline tab, there are 2 types : `1) Pipeline Script` and `2) Pipeline script from SCM(Github)`
- there are samples to get started and once we run/build `hello world` sample code -> In the logs, we can the echo message
                     
                     
           pipeline {
                    agent any

                    stages {
                        stage('Hello') {
                            steps {
                                    echo 'Hello World'
                                   }
                            }
                        }
                    } 
    ![image](https://user-images.githubusercontent.com/35003840/163113486-4ff46e2f-ae57-45aa-a31a-e7d3e20b927b.png)
            
                
            

- We can add multiple stages within one pipeline same like Build->Test->deploy
``` Code
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Build App'
            } }
        stage('Test') {
            steps {
                echo 'Test App'
            } }
        stage('Deploy') {
            steps {
                echo 'Deploy App'
            } }
        }
        
        
        post {
            always{
                emailext body: 'This is the Body response', subject: 'Pipeline Status', to: 'balajisomasale98@gmail.com'
            }
            
        }
            }
```
          
![image](https://user-images.githubusercontent.com/35003840/163118612-1c115c32-6205-4d87-8e34-1ba1d2d6c4c2.png)

4/15:

- Using Failures in Jenkinsfile:                                  

``` Code
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Build App'
            } }
        stage('Test') {
            steps {
                echo 'Test App'
            } }
        stage('Deploy') {
            steps {
                echo 'Deploy App'
            } }
        }
        
        
        post {
            always{
                emailext body: 'This is the Body response', subject: 'Pipeline Status', to: 'balajisomasale98@gmail.com'
            }
        }        
    }

```
- We can Integrate `Pipeline` project type with SCM directly so that first step will be pulling code from SCM ( Github) and then running the stages depending
  syntax.

### Note: When connecting to AWS Ec2 Instance especially Windows Server(Remote desktop Connection), Everytime you need to give the Ec2 public ipv4 DNS, username and password  - It will found here :

- Open the Amazon EC2 console, and then choose Instances. Select the check box for the instance, and then expand the Actions dropdown list. If you're using the old console, then choose Get Windows Password.
- If you're using the new console, choose Security, and then choose Get Windows Password.

For this, EC2 will definetely change but the username and pwd will be constant 
- Username : Administrator
- Pwd : xIzs4f*NtUIlGEvbohR99)xL6Jq7hNEY
# What is Master-Slave configuration:

In any Real-time application,If there are high number of application, the server will act slower so in order to mitigate this, we create slave so that it doesn’t affect the main one at all - If job 1 is master and job 2 is a job both in master and slave : If the job 2 is executed, then automatically the job 2 from slave is built because of master-slave 


Jenkins as a service in windows : we can configure this globally in Jenkins and once we configure it, the only command in Cmd we have to run is “Jenkins.exe start” command to run Jenkins as a service
Same like the services in task manager - we can start and stop the services in the windows services and mainly Jenkins is and can be acts as a windows service!!
Extension : Rebuilder - this helps us to use the same parameter(last) to new Jenkins job
Jenkins CLI - can run the jobs directly from CLI after authentication 

# Docker containers - Jenkins:

### Slack integration with Jenkins



4/17 : 

# Connect Jenkins via AWS Cloud

- Create a EC2 Instance - Unix 16.04 free tier
- Go to key pair pem file location and give command `$ ssh -i Jenkins-key-Ohio.pem ubuntu@3.22.187.170`
- Install JDK in this- command : sudo apt search jdk
- THis command will add repository -> sudo add-apt-repository ppa:openjdk-r/ppa
- For latest version ->  sudo apt update
- Install openjdk 8 version -> sudo apt install openjdk-8-jdk -y
Note: Java jdk 8 and above are accepted and 7 is not compatible anymore

### Installing Jenkins in AWS server:
- In Linux servers -> https://www.jenkins.io/doc/book/installing/linux/
- To become root user : sudo -i
- Follow the steps installation steps in above link
- TO check if the Jenkins is installed or not -> systemctl status jenkins
- To check if its enabled -> systemctl is-enabled jenkins -> you should get "Enabled"
- Command for -> ps -ef | grep jenkins
- To know the process number java -> ss -tunpl | grep 8080


4/24:
---------------------- Jenkins authentication page is not reflecting - issue with port ---
For the above error -> make sure you have `8080` in your security group in Ec2 instance

### Jenkins authentication in AWS server
- Once we go to `http://13.59.192.229:8080` it will redirect to Jenkins authentication homepage 
- here `http://13.59.192.229` is public ipv4 address of ec2 instance
- It will ask us to give the password and it will be found in the mentioned directory itself -> Just go to that directory and use `cat` command to access it
- If permission denied error occurs, then just be the root user `sudo -i`
- Once setting up the profile, it will ask for url of jenkins -> As EC2 is dynamic ( changes after every restart) -> we should change it manually
- If we want Static URL address(public ipv4) - then we need to attach `Elastic Ip` and attach to EC2 instance - we didnot do here 
- Now, `Jenkins is ready and installed`

## Running first job in Jenkins
- As normal, we can create first job by creating new item `my_first_job` and write any shell command as follows;
- `echo "Jenkins job is running with the mentioned user below"
    whoami

    echo "The commands of this job are executed from below directory" 
    pwd 
    touch test_jenkins_files{1..20} `

Creating Build Job in Jenkins: Using Timestamps in the file names
- The following job copies one file to creating a new file in updating the file name as $BUILD_TIMESTAMP 
`cp target/vprofile-v2.war target/vprofile-V$BUILD_ID-$BUILD_TIMESTAMP.war `

 ![image](https://user-images.githubusercontent.com/35003840/165002553-abb74fdf-22c7-414e-9f90-15f629af26c7.png)

##### Install Maven in Jenkins
- Just login into Jenkins in aws with EC2 ipv4 address and become root user `sudo -i` then give `sudo apt install maven`

## Jenkins  AWS Artifact S3 Upload :

- Create a S3 bucket in AWS to upload Files and give access to the user logged in - if not, create a new user with `AmazonS3FullAccess` in the permissions
- Add S3 plugin in Jenkins - `S3 Publisher`
- In Manage System Jenkins > Select Amazon S3 profiles and give the access key and pwds we got when S3 was created!!
- How to link S3 in Jenkins project ? - In post-built actions,select `publish artifacts to S3 bucket`  
- Add the files that needs to be uploaded into AWS - give it in the source field
- In the Destination bucket, Give the S3 bucket name 
- Make sure the AWS S3 bucket region,Ec2 instance region and "Jenkins S3 upload region" should be same - here it is "us-east-2"
![image](https://user-images.githubusercontent.com/35003840/165002473-7f2e3d41-f9e7-4e4b-8cc9-c225597bc25f.png)

Files uploaded in AWS S3 

![image](https://user-images.githubusercontent.com/35003840/165002515-c11d46dd-f1ec-4667-acbf-fff4dcdbb8f2.png)

Job Flow : 
`Creating files in Jenkins --> Uploading the files to AWS S3 Artifact`

Last login : AWS EC2 instance : http://13.59.192.229:8080/

4/29:
What is Artifact in Jenkins?

- The definition of an artifact from Jenkins themselves is: an artifact is an immutable file, generated during a Build or Pipeline run in Jenkins. These artifacts are then archived onto the Jenkins Controller for later use.

### Jenkins Nexus Integration

https://www.sonatype.com/products/nexus-repository

![image](https://user-images.githubusercontent.com/35003840/165964168-16e5fa07-f028-4feb-ae5d-6f89b08f69a2.png)

![image](https://user-images.githubusercontent.com/35003840/165964613-c6149080-8d42-4619-8795-b3b830d0b50d.png)

Flow : Users Code -> Github -> Jenkins -> Nexus repo

![image](https://user-images.githubusercontent.com/35003840/165966228-a6d9e01f-d361-46f7-90b4-fc5f3641e33a.png)

Here, Instead of storing artifact in AWS S3 Bucket, we store the artifacts from jenkins to Nexus Repo

##### How to create Jenkins with Nexus ?

Firstly, we need to create a seperate Nexus Centos server and below are the steps:

- Creating a Centos server in AWS - CentOS 7 (x86_64) - with Updates HVM
- Create everything as default except the Key pair - Create a new Key pair naming 'Nexus-server' here
- So Once the EC2 Centos Instance is created, go to `Nexus-Server.pem` key pair directory and login to that
- command is same : `ssh -i Nexus-Server.pem centos@3.19.213.64` where `Nexus-Server.pem` is the Key pair name and we are using `centos` here
- To become root user to get all permissions: `sudo -i`
- ` Dependencies used are Java` - We need to Install Java - JDK or JRE needs to be installed - `yum search jdk` to search
- We need to install min java 8 version - so look for it - `yum install java-1.8.0-openjdk.x86_64 -y` 

Installing Nexus package: 

- Go to https://help.sonatype.com/repomanager2/download -> we will copy the address of `nexus-2.15.1-02-bundle.tar.gz`
- Make sure to download only the OSS ( open source ) and not the professional one
- Inorder to download the URL, we need to have `wget` and to install : `yum install wget -y`
- To download the URL : `wget https://download.sonatype.com/nexus/oss/nexus-2.15.1-02-bundle.tar.gz`
- Once it is downloaded, move this file : `mv nexus-professional-2.15.1-02-bundle.tar.gz /usr/local/` and  `cd /usr/local/`
- We use `tar -xzvf downloaded_file` to extract it : `tar -xzvf nexus-2.15.1-02-bundle.tar.gz`
- Using `ln` command here : `ln -s nexus-2.15.1-02 nexus`
  Note : `ln command` means : `ln is a command-line utility for creating links between files. By default, the ln command creates hard links. `
- Once we run this command, we get bin folder and once we get into that - there is a binary file named `nexus` which is start script
- To see the webpage, we run this command `bin/nexus start` and we get warning saying to export if its root : `export RUN_AS_USER=root` and try running start again
- Check for logs : `tail -f logs/wrapper.log` - Jetty-server will be started

###### How to login to Nexus server:

- Public ipv4 address and port number 8081: http://3.19.213.64:8081/nexus/
- Sign credentials : `admin` and pwd `admin123` which is default - but changed it to `adminbalu`

Nexus Repository : 

- Check Repositories tab in Nexus server and create new one with `hosted repository` as option
![image](https://user-images.githubusercontent.com/35003840/165990274-a84dc016-d02c-43f1-9add-d91539aa50b4.png)
- Keep everything default as we are just creating artifact here - let `Maven2` be same
- We can see repo here 
![image](https://user-images.githubusercontent.com/35003840/165990479-c55012ea-5fe8-4d4d-8cd8-d65d0bf991e7.png)

##### Install Nexus in Jenkins:

- Add `Nexus Artifact Uploader` plugin in Jenkins and `Copy artifcat plugin` to copy artifact from one project(workspace) to another
- 
- 
 
