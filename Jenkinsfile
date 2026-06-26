pipeline {
    agent any

    tools {
        jdk 'jdk17'
        nodejs 'node16'
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
                sh 'docker build -t starbucks:latest .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag starbucks:latest mohitdevops97/starbucks:latest'
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker', url: '']) {
                        sh 'docker push mohitdevops97/starbucks:latest'
                    }
                }
            }
        }
    }
}
