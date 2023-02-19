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
	  			sh "mkdir -p /zap/wrk"
				sh "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://34.134.183.218/ -r report_html || true"	
	  			
		
		}

	stage ("Publish Reports"){
	       sh "mkdir -p reports"
	       publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'reports', reportFiles: 'report.html', reportName: 'HTMLReport', reportTitles: '', useWrapperFileDirectly: true])
	       
	       }



	
    

        


}
