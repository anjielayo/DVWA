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
  stage('Dependency Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
  
  stage('Trivy Scan') {
            steps {
                script {
                    sh """trivy image --format template --template \"@/home/vijeta1/contrib/html.tpl\" --output trivy_report.html XXXXXXX.dkr.ecr.ap-south-1.amazonaws.com/${params.SERVICE}:${BUILD_NUMBER} """
                    
                }
                
            }
        }


}
