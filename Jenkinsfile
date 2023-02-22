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
	   

	    stage ('Archery with ZAP'){
		    steps {
		  	sshagent(credentials: ['archeryssh']) {
		    		sh 'ssh root@34.122.121.166 archerysec-cli -h http://34.122.121.166:8000 -t v1ARGjtGPbMNZIIMCT7EK0yryuyVpLES3dWSusHBuqi1m1ch6rBXIywjFk-Yv5VN --cicd_id=72e16a6c-fcb3-4505-9da4-491d442e7377 --project=83980117-064a-44b4-8793-c1f1b6e220cf --zap-base-line-scan --report_path=/tmp/archerysec-scans-report/'
		    }
	    }
	    } 
	    
	    
	
	   stage ("Nuclei Scan"){
		    steps {
			    sshagent(credentials: ['wazuhsshfinal']) {
				sh "ssh root@34.121.233.159 docker run projectdiscovery/nuclei:latest -u http://34.134.183.218/"
		    }
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
		  	sh "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://34.134.183.218/ || true"
		 	 }
			}
	    
	    stage ('Chopchop Scan') {
		    steps {
		    	sh "docker run ghcr.io/michelin/gochopchop scan http://34.134.183.218/ -v debug"
		    
		    }
	    
	    }
	    
	    

	   stage('Container Scanning-Trivy Scan') {
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

		stage('XSS Scan-Dalfox Scan') {
			steps {
				sh 'docker run -t hahwul/dalfox:latest /app/dalfox url http://34.134.183.218/'

		}
		}
	
	

	
    

        

    }
}
