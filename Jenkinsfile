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
	  sh "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://34.134.183.218/ || true"
	  	
		}

   stage('Trivy Scan') {
          
              // Scan all vuln levels
                sh 'mkdir -p reports'
                sh 'trivy repo https://github.com/anjielayo/DVWA'
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

	stage('Dalfox Scan'){
		sh 'docker run -it hahwul/dalfox:latest /app/dalfox url http://34.134.183.218/'
	
	}
	
	

	
    

        


}
