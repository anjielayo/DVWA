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
  
  stage ("Dynamic Analysis - DAST with OWASP ZAP") {
			
	  			sh "docker exec mkdir /zap/wrk"
				sh "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://34.134.183.218/ || true"
	  			sh "docker cp owasp:/zap/wrk/report.xml ${WORKSPACE}/report.xml"
			
		
		}

  
    

        


}
