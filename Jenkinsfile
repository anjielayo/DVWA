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
                sh 'trivy repository https://github.com/anjielayo/DVWA.git'
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

	stage('Archery Scan'){
		sh "docker run -it -p 8000:8000 -v http://34.134.183.218:/archerysec archerysec/archerysec:latest"
	
	
	
	}
	
	

	
    

        


}
