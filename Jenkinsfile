pipeline {

    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    environment {

        IMAGE_NAME = 'shafimahmud/portfolio'

        IMAGE_TAG = 'latest'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                        -t ${IMAGE_NAME}:${IMAGE_TAG} \
                        .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry(
                    credentialsId: 'dockerhub',
                    url: 'https://index.docker.io/v1/'
                ) {
                    sh '''
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image successfully pushed to Docker Hub.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}

