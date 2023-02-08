node {

  stage('SCM') {
    checkout scm

  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'Sonartool';
    withSonarQubeEnv('Sonarserver') {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
  
}
