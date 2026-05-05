pipeline{
  agent any
  stages{
    stage('master'){
      when{
        branch 'master'
      }
      steps{
      echo "Building MS pipeline in master branch"
      }
    }
    stage('qa'){
      when{
        branch 'qa'
      }
      steps{
        echo "Building QA Pipeline"
      }
  }
    stage('Notify'){
      steps{
        emailext(
          subject:" Build no:${BUILD_NUMBER} passed - ${JOB_NAME}",
          body: "Build passed in branch: ${BRANCH_NAME}" ,
          to: 'arunallen342@gmail.com'
          )
      }
    }
  }
}
}
