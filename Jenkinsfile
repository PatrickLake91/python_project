pipeline {
  agent any

  environment {
      DOCKERHUB = credentials('DockerHub')
      IMAGE_NAME = "patricklake91/python_project"
  }

  stages {
    stage('Docker Login') {
      steps {
        sh '''
          echo "$DOCKERHUB_PSW" | docker login -u "$DOCKERHUB_USR" --password-stdin
        '''
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          docker build -t $IMAGE_NAME .
        '''
      }
    }

    stage('Run with Docker Compose') {
      steps {
        sh '''
          docker compose down || true
          docker compose up -d
        '''
      }
    }

    stage('Run Tests') {
      steps {
        sh '''
          pytest || true
        '''
      }
    }

    stage('Cleanup') {
      steps {
        sh '''
          docker compose down || true
        '''
      }
    }
  }
}

