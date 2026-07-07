pipeline {
    agent any

    environment {
        GIT_REPO = 'git@github.com:thejasreealuri1808/java11-examples.git'
        GIT_BRANCH = 'master'

        REGISTRY = 'localhost:5000'
        IMAGE_NAME = 'java11-examples'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${GIT_BRANCH}",
                    credentialsId: devopssh
                    url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        docker build -t ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG} .
                        docker tag ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG} ${REGISTRY}/${IMAGE_NAME}:latest
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                    docker push ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
                    docker push ${REGISTRY}/${IMAGE_NAME}:latest
                """
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
