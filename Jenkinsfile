pipeline {
  agent any
  stages {
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t zoroozz/html:latest .'
      }
    }

    stage('Push Docker Image') {
      steps {
        sh '''docker login -u "$DOCKER_USER" -p "$DOCKER_PASS"
docker push zoroozz/html:latest
'''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''kubectl set image deployment/your-deployment your-container=zoroozz/html:latest --record
'''
      }
    }

  }
  environment {
    DOCKER_CRED = 'dockerhub-creds'
  }
}