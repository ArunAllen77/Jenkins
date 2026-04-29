pipeline{
  agent any
  environment{
    App_name= 'Jenkins'
    Version = '1.7'
  }
  parameters{
    choice(name: 'Deployment', choices: ['dev','staging','Production'],description:"Choosing env")
  }
  stages{
    stage('check_out'){
      steps{
        echo " git branch: $GIT_BRANCH"
        echo "commit status: ${env.GIT_COMMIT}"
      }
    }
    stage('test'){
      steps{
        sh '''
        echo "just calling out variables"
        echo "${App_name}:${Version} "
      '''
      }
    }
    stage('docker_test'){
      steps{
        sh 'docker --version'
        sh 'docker ps-a'
        sh 'docker images'
      }
    }
    stage('parameters'){
      steps{
        echo "Deployed on ${params.Deployment}"
      }
    }
  }
}
        
    
