pipeline {
    agent any
    
    tools {
        maven 'local_maven'
    }
     environment {     
         DOCKERHUB_CREDENTIALS= credentials('docker-hub')     
       } 

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    stage('Build Docker Image') {         
      steps{                
	sh 'docker build -t jyotiranswain/k8s:$BUILD_NUMBER .'           
        echo 'Build Image Completed'                
      }           
    }
    stage('Login to Docker Hub') {         
      steps{                            
	sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'                 
	echo 'Login Completed'                
      }           
    }               
    stage('Push Image to Docker Hub') {         
      steps{                            
	sh 'docker push jyotiranswain/k8s:$BUILD_NUMBER'                 
  echo 'Push Image Completed'       
      }           
    }
  stage('Deply to k8s') {         
    steps{
	    sshagent(['kubectl']) {
       sh "scp -o StrictHostKeyChecking=no pod.yml ec2-user@18.141.208.35:/home/ec2-user/kube"
       script{
        try{
          sh "ssh ec2-user@18.141.208.35 kubectl apply -f ."
        }catch(error){
          sh "ssh ec2-user@18.141.208.35 kubectl create -f ."
        }
       }
}
    }       
    }   

  } //stages 
  post{
    always {  
      sh 'docker logout'           
    }      

    }
}
