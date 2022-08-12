pipeline {
    agent any
    
    tools {
        maven 'local_maven'
    }
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.jar'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ("Deploy to Staging"){
                    steps {
                        deploy adapters: [tomcat7(credentialsId: 'bcccb6a4-0208-43a5-8579-be32f8d97d37', path: '', url: 'http://13.234.186.249:8282/')], contextPath: null, war: '**/*.jar'
                    }
                }
            }
        }
    }
}
