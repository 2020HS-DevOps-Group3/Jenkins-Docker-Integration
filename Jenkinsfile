pipeline {
    agent any
  
    environment {
        DOCKERHUB_CREDENTIALS = credentials('generalnitin-dockerhub')
    }

    stages {
        stage('Deploy to Docker Hub') {
            parallel {
                stage('Build Docker') {
                    steps {
                        sh 'docker build . -t generalnitin/docker-jenkins-poc:latest'
                        withCredentials([string(credentialsId: 'generalnitin-dockerhub', variable: 'docker_pwd')]) {
                            sh "docker login -u generalnitin -p ${docker_pwd}"
                        }
                        sh "docker push generalnitin/docker-jenkins-poc:$$BUILD_NUMBER "
                    }
                    post {
                        always {
                            sh 'docker logout'
                        }
                    }
                }
            }
        }
    }
}