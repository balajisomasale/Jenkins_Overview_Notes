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
            
Steps : 
    - In Jenkins/Configure - Firstly give SSH github link here
    - GitHub hook trigger for GITScm polling
    
    
    
   
    

    
    
    
    
    
    
    


