pipeline {
agent any
	stages{
		stage('SCM') {
		  steps {
		    checkout scm
		  }
		}

		  stage('SonarQube Analysis') {
			  def scannerHome = tool 'SonarScanner';
			  steps{
			    
			    withSonarQubeEnv() {
			      sh "${scannerHome}/bin/sonar-scanner"
		    }
		  }
		  }

		  stage ("Dynamic Analysis - DAST with OWASP ZAP") {	
			  steps{
						sh "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://34.134.183.218/ || true"	

			  }
				}

			stage ("Publish Reports"){
				steps{
			       sh "mkdir -p reports"
			       publishHTML([allowMissing: false, 
					    alwaysLinkToLastBuild: false, 
					    includes: '**/*',
					    keepAll: true, 
					    reportDir: 'reports/', 
					    reportFiles: 'report.html', 
					    reportName: 'HTMLReport', 
					    reportTitles: 'report', 
					    useWrapperFileDirectly: true])
				}
			       }






        
	}

}
