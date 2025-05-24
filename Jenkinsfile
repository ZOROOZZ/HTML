pipeline {
  agent any
  stages {
    stage('Build Docker Image') {
      steps {
        sh 'TAG="v$(date +%Y%m%d%H%M%S)"'
        sh 'docker build -t zoroozz/html:$TAG .'
      }
    }

    stage('Push Docker Image') {
      steps {
        sh '''docker login -u "$DOCKER_USER" -p "$DOCKER_PASS"
docker push zoroozz/html:$TAG'''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''kubectl set image deployment/html-app html-app=zoroozz/html:$TAG -n html
'''
      }
    }

  }
  environment {
    DOCKER_CRED = 'dockerhub-creds'
  }
}