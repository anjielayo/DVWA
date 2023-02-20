pipeline {
    agent any 
	environemnt {
		def scannerHome = tool 'SonarScanner';
	}
    stages {
	  stage('SCM') {
		  steps {
		    checkout scm
		  }
	  }
	    
	  stage('SonarQube Analysis') {
		  steps {
		    withSonarQubeEnv('Sonarserver') {
		      sh "${scannerHome}/bin/sonar-scanner"
	    }
	    }
	  }

	  stage ("Dynamic Analysis - DAST with OWASP ZAP") {
		  steps {
		  	sh "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://34.134.183.218/ || true"
		 	 }
			}

	   stage('Trivy Scan') {
		   steps {
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
		    }

		stage('Dalfox Scan') {
			steps {
				sh 'docker run -t hahwul/dalfox:latest /app/dalfox url http://34.134.183.218/'

		}
		}
	
	

	
    

        

    }
}
