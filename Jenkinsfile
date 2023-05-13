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
	       script {
      def dockerRun = "kubectl apply -f /home/ec2-user/kube/pod.yml"
  
	    sshagent(['kubectl']) {
       sh "scp -o StrictHostKeyChecking=no pod.yml ec2-user@18.141.208.35:/home/ec2-user/kube"
       sh "ssh -o StrictHostKeyChecking=no ec2-user@18.141.208.35 ${dockerRun}"
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
