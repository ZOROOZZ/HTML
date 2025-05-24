pipeline {
  agent any
  stages {
    stage('Build Docker Image') {
      steps {
        sh '''TAG="v$(date +%Y%m%d%H%M%S)"
docker build -t zoroozz/html:$TAG .
docker push zoroozz/html:$TAG
kubectl set image deployment/html-app html-app=zoroozz/html:$TAG -n html
kubectl rollout status deployment/html-app -n html'''
      }
    }

  }
  environment {
    DOCKER_CRED = 'dockerhub-creds'
  }
}