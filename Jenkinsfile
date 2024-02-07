pipeline {
    agent any
    tools {nodejs "NodeJs"}
    
    environment {     
    DOCKERHUB_CREDENTIALS= credentials('dockerHub') 
     SCANNER_HOME = tool 'sonar-scanner';  
       } 

    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/kalyanisalalkar/node-todo.git", branch: "main"
                echo 'bhaiyya code clone ho gaya'
            }
        }
        stage("build and test"){
            steps{
                sh "docker build -t node-app-test-new ."
                echo 'code build bhi ho gaya'
            }
        }
       
        stage('SonarQube analysis') {
           
            steps {
                withSonarQubeEnv('sonar-server') {
                sh "${SCANNER_HOME}/bin/sonar-scanner"
             
              }
             
            }
        }
        stage('Login to Docker Hub') {      	
        steps{                       	
	           sh 'echo $DOCKERHUB_CREDENTIALS_PSW'
	           sh 'docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW' 
	           sh "docker tag node-app-test-new:latest $DOCKERHUB_CREDENTIALS_USR/node-app-test-new:latest"
               sh "docker push $DOCKERHUB_CREDENTIALS_USR/node-app-test-new:latest"
            	echo 'Login Completed'      
               }           
         }   
       stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo 'deployment ho gayi'
            }
        }
    }
}
