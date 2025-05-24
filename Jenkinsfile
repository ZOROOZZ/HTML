pipeline {
  agent any
  stages {
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t zoroozz/html:v2 .'
      }
    }

    stage('Push Docker Image') {
      steps {
        sh '''docker login -u "$DOCKER_USER" -p "$DOCKER_PASS"
docker push zoroozz/html:v2
'''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''kubectl set image deployment/html-app html-app=zoroozz/html:v2 -n html
'''
      }
    }

  }
  environment {
    DOCKER_CRED = 'dockerhub-creds'
  }
}