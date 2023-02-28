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
	    
	    
	stage('Container Scanning-Trivy Scan') {
		   steps {
		      // Scan all vuln levels
			sh 'mkdir -p reports'
			sh 'trivy repo https://github.com/knqyf263/trivy-ci-test,library --format template --template "@html.tpl" -o reports/trivyscan.html ./nodej'
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
	    
	    
         stage('Secrets Management-GitGuardian Scan') {
            agent {
                docker { image 'gitguardian/ggshield:latest'
		       args '-i --entrypoint='}
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
		  	sh "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://34.134.183.218/ || true"
		 	 }
			}
	    
	    stage ('Chopchop Scan') {
		    steps {
		    	sh "docker run ghcr.io/michelin/gochopchop scan http://34.134.183.218/ -v debug"
		    
		    }
	    
	    }
	    
	     stage ("Nuclei Scan"){
		    steps {
			    
				sh "docker run projectdiscovery/nuclei:latest -u http://34.134.183.218/"
		    
		    }
	    }

	   

		stage('XSS Scan-Dalfox Scan') {
			steps {
				sh 'docker run -t hahwul/dalfox:latest /app/dalfox url http://34.134.183.218/'

		}
		}
	
	

	
    

        

    }
}
