node {
    environment {
      // SEMGREP_BASELINE_REF = ""

        SEMGREP_APP_TOKEN = 'cf7734121f06fa24f28b324a7ecc67124b914e82dea64050d8158ff885cc531e'
       

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
      
          sh 'pip3 install semgrep'
          sh 'semgrep login'
          sh 'semgrep ci'
      }
    
  
    

        


}
