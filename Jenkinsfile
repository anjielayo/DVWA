pipeline {
    agent any 
	environment {
		def scannerHome = tool 'SonarScanner';
		SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
        	SEMGREP_PR_ID = "${env.CHANGE_ID}"
		GITGUARDIAN_API_KEY = credentials('gitguardian-api-key')
		GITHUB_AUTH_TOKEN = credentials('GITHUB_AUTH_TOKEN')
		
	}
	
    stages {
	  stage('SCM') {
		  steps {
		    checkout scm
		  }
	  }
	    
	    
	   stage('Secrets Management-GitGuardian Scan') {
            agent {
                docker { image 'gitguardian/ggshield:latest' }
            }
            steps {
                sh 'ggshield secret scan ci'
            }
        } 
	    

	  stage('SAST Analysis-SonarQube') {
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
	    
	    
	  stage ("Dynamic Analysis - OWASP ZAP") {
		  steps {
		  	sh "docker run -t owasp/zap2docker-stable zap-full-scan.py -t https://www.vacasa.com/ || true"
		 	 }
			}
	    
	    stage ('Chopchop Scan') {
		    steps {
		    	sh "docker run ghcr.io/michelin/gochopchop scan https://www.vacasa.com/ -v debug"
		    
		    }
	    
	    }
	    
	     stage ("Nuclei Scan"){
		    steps {
			    sshagent(credentials: ['wazuhsshfinal']) {
				sh "ssh root@34.121.233.159 docker run projectdiscovery/nuclei:latest -u https://www.vacasa.com/"
		    }
		    }
	    }

	   stage('Container Scanning-Trivy Scan') {
		   steps {
		      // Scan all vuln levels
			sh 'mkdir -p reports'
			sh 'trivy repo http://34.134.183.218/'
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

		stage('XSS Scan-Dalfox Scan') {
			steps {
				sh 'docker run -t hahwul/dalfox:latest /app/dalfox url https://www.vacasa.com/'

		}
		}
	
	

	
    

        

    }
}
