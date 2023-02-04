pipeline {
    agent any
    environment {
        registry = "675391935708.dkr.ecr.ap-south-1.amazonaws.com/aquila"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/abhijeetatmindstix/node-todo-cicd.git']]])     
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 675391935708.dkr.ecr.ap-south-1.amazonaws.com'
                sh 'docker push 675391935708.dkr.ecr.ap-south-1.amazonaws.com/aquila:latest'
         }
        }
      }
   
         // Stopping Docker containers for cleaner Docker run
     stage('stop previous containers') {
         steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
      
    stage('Docker Run') {
     steps{
         script {
                sh 'docker run -d -p 8096:8000 --rm --name mypythonContainer 675391935708.dkr.ecr.ap-south-1.amazonaws.com/aquila:latest'
            }
      }
    }
    }
}
