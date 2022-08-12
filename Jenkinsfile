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
                       sh "scp -o StrictHostKeyChecking=no **/*.war ec2-user@13.126.48.59:/opt/tomcat/webapps/"
      }
                    }
                }
            }
        }
    }
}
