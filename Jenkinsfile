pipeline{
  agent any
  stages{
    stage('checkout'){
      steps{
        echo "Branch: ${BRANCH_NAME}
        echo "Jobname: ${JOB_NAME}
      }
    }
    stage('Sonarqube Analysis'){
      steps{
        withSonarQubeEnv('Sonarqube'){
          sh '''
          sonar-scanner \
          -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
          -Dsonar.sources=. \
          -Dsonar.host.url=http://sonarqube:9000
          '''
        }
      }
    }
    stage('Quality Gates'){
      steps{
        timeout(time:2,unit: 'MINUTES'){
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
