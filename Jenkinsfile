pipeline{
  agent any
  environment{
    SONAR_PROJECT_KEY = 'Python-App'
  }
  stages{
    
    stage('checkout'){
      steps{
          echo "Branch: ${env.BRANCH_NAME} "
        echo "Jobname: ${JOB_NAME} "
      }
    }
    stage('Sonarqube Analysis') {
    steps {
        withSonarQubeEnv('Sonarqube') {
            script {
                def scannerHome = tool 'SonarScanner'
                sh """
                    ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=Python-App \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://sonarqube:9000
                """
            }
        }
    }
}
    stage('Quality Gates'){
      steps{
        timeout(time:12,unit: 'MINUTES'){
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
        post {
        success {
            emailext(
                subject: "✅ Build #${BUILD_NUMBER} Passed — ${JOB_NAME}",
                body: "Branch: ${env.BRANCH_NAME}\nBuild: ${BUILD_URL}",
                to: 'arunallen342@gmail.com'
            )
        }
        failure {
            emailext(
                subject: "❌ Build #${BUILD_NUMBER} Failed — ${JOB_NAME}",
                body: "Branch: ${env.BRANCH_NAME}\nBuild: ${BUILD_URL}",
                to: 'arunallen342@gmail.com'
            )
        }
    }
}
