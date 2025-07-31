pipeline {
  agent any

  environment {
    REGISTRY = "localhost:5000"
    IMAGE = "flaskapp"
    TAG = "${env.BUILD_NUMBER}"
  }

  stages {
    stage('Clone Repo') {
      steps {
        git 'https://github.com/<your-username>/<your-repo>.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
        docker build -t ${REGISTRY}/${IMAGE}:${TAG} .
        docker tag ${REGISTRY}/${IMAGE}:${TAG} ${REGISTRY}/${IMAGE}:latest
        '''
      }
    }

    stage('Push to Registry') {
      steps {
        sh '''
        docker push ${REGISTRY}/${IMAGE}:${TAG}
        docker push ${REGISTRY}/${IMAGE}:latest
        '''
      }
    }
  }
}

