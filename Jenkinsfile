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
	  			sh "docker run -v $(pwd):/zap/wrk/:rw -t owasp/zap2docker-stable zap-baseline.py \ -t http://34.134.183.218/ -g gen.conf -r testreport.html || true"
				
	  			
		
		}

   stage('Trivy Scan') {
          
              // Scan all vuln levels
                sh 'mkdir -p reports'
                sh 'trivy repository --ignore-unfixed --vuln-type os,library --format template --template "@html.tpl" -o reports/trivyscan.html https://github.com/anjielayo/DVWA.git'
                publishHTML target : [
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'trivyscan.html',
                    reportName: 'Trivy Scan',
                    reportTitles: 'Trivy Scan'
                ]


            }



	
    

        


}
