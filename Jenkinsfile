pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t devops-demo/frontend:3.0 .'
            }
        }

        stage('GHCR Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'github-ghcr',
                    usernameVariable: 'GHCR_USER',
                    passwordVariable: 'GHCR_TOKEN'
                )]) {
                    bat 'echo %GHCR_TOKEN% | docker login ghcr.io -u "%GHCR_USER%" --password-stdin'
                }
            }
        }

        stage('Docker Tag') {
            steps {
                bat 'docker tag devops-demo/frontend:3.0 ghcr.io/devops-cicd-demo1/frontend:3.0'
            }
        }

        stage('Docker Push') {
            steps {
                bat 'docker push ghcr.io/devops-cicd-demo1/frontend:3.0'
            }
        }

    }
}
