# Jenkins_SampleFirstProject


How to run jenkins after installation ??

Go to jenkins.war folder location and run "java -jar jenkins.war" and go to the URL -> http://localhost:8080/


If it shows Unsupported Java version, Install it to Java 11 jdk version -> https://www.oracle.com/in/java/technologies/javase-jdk11-downloads.html
Note: Java 17 is not supported 


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




