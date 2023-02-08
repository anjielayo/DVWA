pipeline {
  agent any
  stages {
    stage('SonarQube analysis') {
      steps {
        script {
          // requires SonarQube Scanner 2.8+
          scannerHome = tool 'SonarQube'
        }
        withSonarQubeEnv('Sonarqube') {
          sh "${scannerHome}/bin/sonar-scanner"
        }
      }
    }
  }
}
