# Deploy on a Nginx server
# *Jenkins Job name:* 'Deploy_on_Nginx_Server'

## Pre-requisites

1. Jenkins server 
2. Nginx Server 

## Adding Deployment steps

1. Install 'deploy to container' plugin. This plugin needs to deploy on tomcat server. 

  - Install 'deploy to container' plugin without restart ' 
    - 'Manage Jenkins' > 'Jenkins Plugins' > 'available' > 'deploy to container'
 
2. Jenkins should need access to the Nginx server to deploy build artifacts. setup credentials to enable this process. use credentials option on Jenkins home page.

- setup credentials
  - 'credentials' > 'jenkins' > 'Global credentials' > 'add credentials'
    - Username	: 'deployer'
    - Password : 'deployer'
    - id      :  'deployer'
    - Description: 'user to deploy on tomcat vm'

### Steps to create "Deploy_on_Nginx_Server" Jenkin job
 #### From Jenkins home page select "New Item"
   - Enter an item name: 'Deploy_on_Nginx_Server'
     - Copy from: `My_First_Maven_Build`
     
   - *Source Code Management:*
      - Repository:  'https://gitlab.com/jc1arke/tweet-interpreter'
                   
                   
      - Branches to build : '*/master'  
   - *Poll SCM* :      - '* * * *'

   - *Build:*
     - Root POM:'pom.xml'
     - Goals and options: 'clean install package'

 - *Post-build Actions*
   - Deploy war/ear to container
      - WAR/EAR files :'`**/*.war'
      - Containers : 'Nginx'
         - Credentials: 'deployer' (user created on above)
         - Nginx URL : 'http://<PUBLIC_IP>:8080'

Save and run the job now.