pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="675391935708"
        AWS_REGION = "ap-south-1"
        REPO_NAME="aquila"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${REPO_NAME}"
    }
   
    stages {
         // Downloading the source file and logging into ECR
         stage('Logging into AWS ECR') {
            steps {
                script {
                
                sh 'https://github.com/abhijeetatmindstix/node-todo-cicd.git'
                sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                }
                 
            }
        }
        
  
    // Building Docker images
    stage('Building image') {
      steps{
            sh 'docker build . -t node-app-todo'
            sh "docker tag aquila-image:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${REPO_NAME}:latest"
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${REPO_NAME}:latest"
         }
        }
      }
    }
}
