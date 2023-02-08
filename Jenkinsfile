
node {

  stage('SCM') {
    checkout scm
    sh ‘node build.js -kvv 9.3.11’
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarQube';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
  
}
