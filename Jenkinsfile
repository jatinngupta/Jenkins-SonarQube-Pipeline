node('master') {
    def myMavenContainer = docker.image('maven')
    myMavenContainer.pull()

    stage('checkout scm') {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: ${githubCreds}, url: ${env.GIT_URL}]]])
    }

    stage('maven build') {
      myMavenContainer.inside() {
        sh 'mvn clean install'
      }
    }

    stage('sonar-scanner analysis') {
      def sonarqubeScannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
      withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
        sh "${sonarqubeScannerHome}/bin/sonar-scanner -X -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=${env.JOB_NAME} -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=${env.JOB_BASE_NAME} -Dsonar.sources=src/main/java -Dsonar.java.libraries=target/* -Dsonar.java.binaries=target/classes -Dsonar.language=java"
      }
    }                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
}
