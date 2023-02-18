node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
   stage('Snyk Test') {
      
        echo 'Testing...'
        snykSecurity(
          snykInstallation: 'snyktool',
          snykTokenId: 'first-snyk-token',
          // place other parameters here
        )
      }
    

        


}
