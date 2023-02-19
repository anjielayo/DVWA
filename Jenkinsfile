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


// This stage is ready to use code block for capturing the console output to a txt file.
// Change the ProjectName and buildConsolelog file name as per your requirement

def directory = "${env.WORKSPACE}/ProjectName" // change name here
stage('capture console output') {

  script {
    def logContent = Jenkins.getInstance().getItemByFullName(env.JOB_NAME).getBuildByNumber(
      Integer.parseInt(env.BUILD_NUMBER)).logFile.text
    // copy the log in the job's own workspace
    writeFile file: directory + "/buildConsolelog.txt",
      text: logContent

  }
  def consoleOutput = readFile directory + '/buildConsolelog.txt'
  echo 'Console output saved in the buildConsolelog file'
  echo '--------------------------------------'
  echo consoleOutput
  echo '--------------------------------------'

}
	
    

        


}
