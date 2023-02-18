node {
    environment {
      // SEMGREP_BASELINE_REF = ""

        SEMGREP_APP_TOKEN = '${{ secrets.SEMGREP_APP_TOKEN }}'
       

      //  SEMGREP_TIMEOUT = "300"
    }
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
  
  stage('Semgrep-Scan') {
      
         
          sh 'semgrep ci --config'
      }
    
  
    

        


}
