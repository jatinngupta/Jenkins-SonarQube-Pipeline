# Jenkins-SonarQube-Pipeline

This repository will guide you to setup sonarqube with jenkins inside Docker.

Let's start.

1. Clone this repo,
   git clone https://github.com/Jating7413/Jenkins-SonarQube-Pipeline.git
2. cd docker/
3. make build
4. cd ..
5. docker-compose up -d

To check logs,
6. docker-compose logs -f

This will start create a docker image of Jenkins with the latest version and install required docker libraries inside of it. Docker-compose file will start 3 containers, one for Jenkins, one for SonarQube server and one for Postgres database with which the sonarqube will interact to store reports.

Before running the SonarQube Analysis, first install some required plugins in Jenkins

Open Jenkins Console at http://localhost:8080
Login and Click on Manage Jenkins -> Manage Plugins

Install the following Plugins,
SonarQube-Scanner
Pipeline Utility Steps
httpRequest

After installing the Plugins, go to Manage Jenkins -> Configure Global Tool Configuration
Here add SonarQube Scanner Installation and give it name "sonar-scanner" ( Give sonar-scanner as it is configured in Jenkinsfile)

After this we need to register the Sonarqube server token in jenkins,
Open sonarqube at http://localhost:9000

Here it will ask you for integration, name it anything and copy the token.

Open jenkins again,
Go to credentials, add new credentials -> Select secret -> Name it sonar and paste the token -> Click on save.


We are now ready to go,

Create a new Jenkins pipeline job and paste the Jenkinsfile contents over there.
Click on build now.

This job will launch a maven container, pull the source code of an example pet-clinic spring project, do the sonarqube analysis and publish the results to sonarqube server, will check for the Quality gate that whether the build has passed the sonarqube analysis or not.
