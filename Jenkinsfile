pipeline {
    agent any 
	environment {
		def scannerHome = tool 'SonarScanner';
		SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
        	SEMGREP_PR_ID = "${env.CHANGE_ID}"
		GITGUARDIAN_API_KEY = credentials('gitguardian-api-key')
	}
    stages {
	  stage('SCM') {
		  steps {
		    checkout scm
		  }
	  }
	    
	    
	   stage('GitGuardian Scan') {
            agent {
                docker { image 'gitguardian/ggshield:latest' }
            }
            steps {
                sh 'ggshield secret scan ci'
            }
        } 
	    
	    stage('OSS and Dependency Management'){
		    steps {
			sh "docker run gcr.io/openssf/scorecard:stable --repo=https://github.com/anjielayo/DVWA"
		    }
	    }
	    
	  stage('SonarQube Analysis') {
		  steps {
		    withSonarQubeEnv('Sonarserver') {
		      sh "${scannerHome}/bin/sonar-scanner"
	    }
	    }
	  }

	  stage('Semgrep-Scan') {
		  steps {
		    sh 'pip3 install semgrep'
		    sh 'semgrep ci'
		  }
      }  
	    
	    stage ('Chopchop Scan') {
		    steps {
		    	sh "docker run ghcr.io/michelin/gochopchop scan http://34.134.183.218/ -v debug"
		    
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
