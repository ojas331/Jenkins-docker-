
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ojas12345/jenkins-docker-cicd'
        CONTAINER_NAME = 'jenkins-docker-app'
    }

    stages {
        stage('Pull Code from GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$BUILD_NUMBER .'
                sh 'docker tag $DOCKER_IMAGE:$BUILD_NUMBER $DOCKER_IMAGE:latest'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_TOKEN'
                    )
                ]) {
                    sh '''
                        set +x
                        echo "$DOCKER_TOKEN" | docker login \
                            -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKER_IMAGE:$BUILD_NUMBER
                        docker push $DOCKER_IMAGE:latest
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d \
                        --name $CONTAINER_NAME \
                        --restart unless-stopped \
                        -p 8081:80 \
                        $DOCKER_IMAGE:$BUILD_NUMBER
                '''
            }
        }

        stage('Verify Application') {
            steps {
                sh '''
                    sleep 3
                    docker ps --filter "name=$CONTAINER_NAME"
                    curl --fail http://localhost:8081/
                '''
            }
        }
    }
}




