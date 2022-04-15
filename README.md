# Jenkins_SampleFirstProject


How to run jenkins after installation ??

Go to jenkins.war folder location and run "java -jar jenkins.war" and go to the URL -> http://localhost:8080/


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


# 2 Imp Plugins  - Parameterized Trigger and Build Pipeline View

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
                            }

![image](https://user-images.githubusercontent.com/35003840/163118612-1c115c32-6205-4d87-8e34-1ba1d2d6c4c2.png)

4/15:

- Using Failures in Jenkinsfile:

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




