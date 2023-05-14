pipeline {
    agent { 
	    label 'kubectl' 
    }
    
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
	kubeconfig(caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMvakNDQWVhZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJek1EVXhNakUyTVRBME9Wb1hEVE16TURVd09URTJNVEEwT1Zvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBSytjClNpdGx6T3pRa1AwaTBwbERUclVFSmtDdVdIV0xTM0Z2dkNGeFBnS1M0cXMxbVBJUTNFR0ZaZm9Ebi9ZR21KTlYKdzFlR3pRME5OSVAxYUFrdUVYeC9OUXlENElWeit2cldZUnpNaGZVSjJTaVJrdFlTRzlMTEZSY3ZFVVJOT3dscQp6TVgzUUl5Ri85RHJPZlFzdlAxWDNlMTNNVEJhS3RKRlZaSnRJT3JwNHByTXJOM2ExalpOY3BGL09wYlMzbGtTCnJzR1hZZ2hlV3JIU282WGZSTjA0VzgzbEhLWjFoNk5lOVlLNmo5cEFxOUFUTzkzSWN3Z3dHK2RXOHhHeDFqRUIKenY0SUduWGVVcHg3Znd1cjA4V2J4QzZJOTZhV0hMYnZ6YTFFenpqS1VCaEJWWWpJOVZNdC9BQUpiZjU5bm80VApjVVphV1FreGFYU1ZEMnRERE1VQ0F3RUFBYU5aTUZjd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZKWExlbW1wdkZnSVlpWElpRSttMzhXdGMwR09NQlVHQTFVZEVRUU8KTUF5Q0NtdDFZbVZ5Ym1WMFpYTXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBSmthY0JLakw4Nmo0VWNjbW0rZgp6UjMvRWVhVXRkM01JcllZVTFlS0w3MkxEd3VvSE1rODBGY2U4Z2ZZMnBaaUJZM3R5RDQvTjV4RXkzd0wxdURUCldrSXRPMytqRkdMRHVqTTlNT29id2JLVTVlcFVEZ3ZRcElKOVNtT3JsaElFeVNsZzd1RWxjVDB0dWNPY0NLVkUKUFFwd3hycWxYOE1DN0ZDMHQ3ZkVCRnZKSzg2SkJicHlqbGxsNEREWlAySUI5U1REOGVoVVhWOWMwRnlaTWpQVAp6UVpReFNGZWRNZXpGL1BjdFR5cVNXcnVLZy90aTFjVkxXY3lISE9sV0JHSUNlNFcvYWNFWGZkQlQrMFl0QjV1CmVhL01rSkZ2ZCsvZmJVL2pNeEQvdjZmdlpiTW54bjBGSzJmT25pdSttdGJpaDQ5Y1ZiUDJWckZ3a2ZJK2JlamMKNnRVPQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==', serverUrl: 'https://466AB714AE7E706A9FE21FDEFC3CF593.gr7.ap-southeast-1.eks.amazonaws.com') {
    sh 'kubectl apply -f pod.yml'
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
