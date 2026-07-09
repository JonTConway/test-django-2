pipeline{
  agent any
  stages{
    stage('checkout'){
      steps{
        git branch: 'main', url: 'https://github.com/JonTConway/test-django-2'
      }
    }
    stage('Login to ECR'){
      steps{
        withAWS(region: 'us-east-1', credentials: 'aws-creds'){
          powershell '''
          $password = aws ecr get-login-password --region us-east-1
          docker login --username AWS --password $password 590183736441.dkr.ecr.us-east-1.amazonaws.com
          '''
        }
      }
    }
    stage('Build docker image'){
      steps{
        powershell '''
        docker build -t test:django .
        docker tag test:django 590183736441.dkr.ecr.us-east-1.amazonaws.com/test:django
        '''
      }
    }
    stage('Pushing image to ECR'){
      steps{
        powershell '''
        docker push 590183736441.dkr.ecr.us-east-1.amazonaws.com/test:django
        '''
      }
    }
  }
}
