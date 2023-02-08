
node {

  stage('SCM') {
    checkout scm

  }
  stage('SonarQube analysis') {
    def scannerHome = tool 'SonarQube';
    withSonarQubeEnv('Sonarqube') { // If you have configured more than one global server connection, you can specify its name
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
  


}
stage('SQuality Gate') {
     
       timeout(time: 1, unit: 'MINUTES') {
       waitForQualityGate abortPipeline: true
       
  }
}
