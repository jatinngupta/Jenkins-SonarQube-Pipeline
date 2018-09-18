node('master') {
    def myMavenContainer = docker.image('maven')
    myMavenContainer.pull()

    stage('checkout scm') {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/spring-projects/spring-petclinic.git']]])
    }

    stage('maven build') {
      myMavenContainer.inside() {
        sh 'mvn clean install -Dmaven.test.skip=true'
      }
    }

    stage('sonar-scanner analysis') {
      def sonarqubeScannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
      withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
        sh "${sonarqubeScannerHome}/bin/sonar-scanner -X -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=${env.JOB_NAME} -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=${env.JOB_BASE_NAME} -Dsonar.sources=src/main/java -Dsonar.java.libraries=target/* -Dsonar.java.binaries=target/classes -Dsonar.language=java"
      }
    sh "sleep 40"
    env.WORKSPACE = pwd()
    def file = readFile "${env.WORKSPACE}/.scannerwork/report-task.txt"
    echo file.split("\n")[5]
    
    def resp = httpRequest file.split("\n")[5].split("l=")[1]
    
    ceTask = readJSON text: resp.content
    echo ceTask.toString()
    
    def response2 = httpRequest url : 'http://sonarqube:9000' + "/api/qualitygates/project_status?analysisId=" + ceTask["task"]["analysisId"]
    def qualitygate =  readJSON text: response2.content
    echo qualitygate.toString()
    if ("ERROR".equals(qualitygate["projectStatus"]["status"])) {
        echo "Build Failed"
    }
       else {
        echo "Build Passed"
    //   build_job("Trigger the delivery job")		
  }
 }


}
// }
