pipeline {
    agent none

    environment {
        DOCKER_IMAGE = 'zoroozz/html:latest'
    }

    stages {
        stage('Clone Repository') {
            agent any
            steps {
                git 'https://github.com/ZOROOZZ/HTML.git'
            }
        }

        stage('Build Docker Image') {
            agent any
            steps {
                script {
                    echo 'Building Docker image...'
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push to Docker Hub') {
            agent any
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        echo 'Logging into Docker Hub...'
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                        sh 'docker push $DOCKER_IMAGE'
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            agent any
            steps {
                script {
                    echo 'Deploying to Kubernetes...'
                    sh 'kubectl set image deployment/html-app html-app=$DOCKER_IMAGE -n html'
                    sh 'kubectl rollout restart deployment/html-app -n html'
                }
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed. Please check the error log above.'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
    }
}
