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
    stage('SonarQube analysis') {
        steps {
    withSonarQubeEnv('sonarqube-9.8') { 
      // You can override the credential to be used
      sh 'mvn clean verify sonar:sonar \
  -Dsonar.projectKey=demo-project \
  -Dsonar.host.url=http://13.232.158.245:9000 \
  -Dsonar.login=sqp_ec8638cc383ef13e6be8b950592b51af932363c2'
    }
    }
  }

        stage ('Deployments'){
                             steps {
                       echo 'Deploy success'
                    }
                }
    }
}
