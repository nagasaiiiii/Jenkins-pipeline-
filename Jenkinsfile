pipeline {
    agent any
  
      tools {
        maven 'MAVEN_HOME'
    }
    environment {
        // Docker credentials defined in Jenkins credentials store
        DOCKER_CREDENTIALS_ID = 'docker_credentials'
        DOCKER_REGISTRY = 'jittu42779'
        DOCKER_IMAGE = 'addressbook'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Ravi4090/addressbook.git'
            }
        }
        stage('Maven Build') {
            steps {
                script {
                    // Ensure Maven is installed on your Jenkins agent
                    sh 'mvn clean package'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Building Docker image using Dockerfile in the repo
                    sh "docker build -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker_credentials', passwordVariable: 'docker_password', usernameVariable: 'docker_username')]) {
                        sh "echo ${docker_password} | docker login -u ${docker_username} --password-stdin"
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }
}
