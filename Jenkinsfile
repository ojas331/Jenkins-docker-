
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'GitHub connection successful!'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Docker image will be built here.'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Docker image will be pushed here.'
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Application will be deployed here.'
            }
        }
    }
}


