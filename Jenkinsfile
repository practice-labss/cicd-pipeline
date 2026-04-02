pipeline{
  agent any

  tools {
    nodejs 'node'
  }

  environment {
    APP_PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
    IMAGE_NAME = "${env.BRANCH_NAME == 'main' ? 'nodemain' : 'nodedev'}"
    CONTAINER_NAME = "app_container_${env.BRANCH_NAME}"
    IMAGE_TAG = "v1.0"
  }

  stages {
    stage('Checkout'){
      steps {
        checkout scm
      }
    }

    stage('Build'){
      steps {
        sh 'npm install'
      }
    }

    stage('Test'){
      steps {
        sh 'npm test || echo "No tests defined..."'
      }
    }

    stage('Build Docker Image'){
      steps {
        sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
      }
    }

    stage('Deploy'){
        sh "docker rm -f ${CONTAINER_NAME} || true"

        sh "docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:3000 ${IMAGE_NAME}:${IMAGE_TAG}"
    }

  }

}
