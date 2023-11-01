pipeline {
    agent any
    
    tools {
        maven 'local_maven'
    }
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
        }

        stage ('Deployments'){
                    steps {
                       echo 'depploy'
                    }
                }
            }
        }
