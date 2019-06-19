pipeline {
    agent any
    stages {
        stage('Build Application'){
            steps {
                sh "mvn clean compile"
            }
        }
        stage('Tests'){
            steps {
                sh "mvn test"
            }
        }
        stage('Package'){
            steps {
                sh "mvn install -DskipTests"
            }
        }
        stage('Build Image Docker'){
            steps {
                sh "sudo docker build -t event-manager ."
            }
        }
        stage('Deploy'){
            steps {
                script {
                  try {
                      sh 'sudo docker rm -f EventManagerAPI'
                  } catch (Exception e) {
                      echo 'Handle the exception!'
                  }
                  sh "sudo docker run --name EventManagerAPI -p 50001:9090 -d event-manager"
                }
            }
        }
    }

    post {
        cleanup {
            // leave workspace clean after build
            archiveArtifacts 'target/*.jar'
            junit 'target/surefire-reports/*.xml'
            cleanWs()
        }
    }
}
