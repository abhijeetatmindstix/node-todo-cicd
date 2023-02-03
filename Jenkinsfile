pipeline {
    
    agent any
    
    stages{
        stage('Code'){
            steps{
                git url: 'https://github.com/abhijeetatmindstix/node-todo-cicd.git', branch: 'master' 
            }
        }
        stage('Build and Test'){
            steps{
                sh 'docker build . -t trainwithshubham/node-todo-test:latest'
            }
        }
        
        stage('container-creation'){
            steps{
                sh 'docker run -d --name node-app-container -p 8000:8000 node-app-todo'
            }
        }
        
        stage('Push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	     sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                 sh 'docker push trainwithshubham/node-todo-test:latest'
                }
            }
        }
        stage('Deploy'){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
        stage('Congratulations'){
            steps{
                echo "Application is Running"
            }
        }        
    }
}
