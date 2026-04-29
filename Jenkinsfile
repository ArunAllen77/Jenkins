pipeline{
  agent any
  environment{
    ECR_REGISTRY='448398088949.dkr.ecr.ap-southeast-2.amazonaws.com/python-app'
    IMAGE_NAME='python-app'
  }
  stages{
    stage('build'){
      steps{
      sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} . '
    }
    }
    stage('Debug Credentials') {
    steps {
        withCredentials([
            string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY'),
            string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_KEY')
        ]) {
            sh '''
                echo "Access key length: ${#AWS_ACCESS_KEY}"
                echo "Secret key length: ${#AWS_SECRET_KEY}"
            '''
        }
    }
}
    stage('ECR_PUSH'){
      steps{
        withCredentials([
          string(credentialsId:'aws-access-key' , variable:'AWS_ACCESS_KEY') , 
          string(credentialsId:'aws-secret-key' , variable:'AWS_SECRET_KEY')
          ])
        {
      sh '''
        aws configure set aws_access_key_id $AWS_ACCESS_KEY
        aws configure set aws_secret_access_key $AWS_SECRET_KEY
        aws configure set region ap-southeast-2

        aws ecr get-login-password --region ap-southeast-2 | docker login --username AWS --password-stdin 448398088949.dkr.ecr.ap-southeast-2.amazonaws.com
        docker tag python-app:latest 448398088949.dkr.ecr.ap-southeast-2.amazonaws.com/python-app:${BUILD_NUMBER}
        docker push 448398088949.dkr.ecr.ap-southeast-2.amazonaws.com/python-app:${BUILD_NUMBER}

      '''
        }
  }
    }
  }
  post{
    success{
      echo "Pipeline passed with ${IMAGE_NAME}: ${BUILD_NUMBER}"
    }
    failure{
      echo "Pipeline Failed"
}}
  
}
