pipeline {
    agent none
    stages {
        stage('Clone Repository') {
            agent any
            steps {
                git 'https://github.com/ZOROOZZ/HTML.git'
            }
        }
        
        stage('Build Docker Image') {
            agent { label 'docker-cloud' }
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t zoroozz/html:latest .'
                }
            }
        }
        
        stage('Push to Docker Hub') {
            agent { label 'docker-cloud' }
            steps {
                script {
                    // Push to Docker Hub
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker push zoroozz/html:latest'
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            agent { label 'MyKubernetesCloud' }
            steps {
                script {
                    // Deploy to Kubernetes
                    sh 'kubectl set image deployment/node-app node-app=zoroozz/html:latest'
                    sh 'kubectl rollout restart deployment/node-app'
                }
            }
        }
    }
}
