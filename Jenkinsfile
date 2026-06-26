pipeline {
    agent any

    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }

    environment {
        DOCKER_IMAGE = 'mohitdevops97/starbucks'
        IMAGE_TAG    = 'latest'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/CloudDevOpsHub/Starbucks-Application.git'
            }
        }

        stage('Install NPM Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build with the final tag directly — no separate tag stage needed
                sh "docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', url: 'https://index.docker.io/v1/') {
                        sh "docker push ${DOCKER_IMAGE}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Image pushed successfully: ${DOCKER_IMAGE}:${IMAGE_TAG}"
        }
        failure {
            echo "❌ Pipeline failed — check Docker Hub credentials or build logs."
        }
        always {
            sh "docker image prune -f"  // cleanup dangling/local images to save disk
        }
    }
}
