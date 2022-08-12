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
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ("Deploy to Staging"){
                    steps {
                        sshagent(['b0962c0e-e3d6-4ff1-8cca-7a282bad7546']) {
                       sh "scp -o StrictHostKeyChecking=no **/*.war ec2-user@13.234.186.249:/opt/tomcat/webapps/"
      }
                    }
                }
            }
        }
    }
}
