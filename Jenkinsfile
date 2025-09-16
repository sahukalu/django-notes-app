pipeline {
  agent { label 'jenkins-agent' }   // or 'dev' if you fixed labels
  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/sahukalu/django-notes-app.git', branch: 'main'
      }
    }

    stage('Build') {
      steps {
        echo "🔧 Building Docker image"
        sh 'docker compose build'
      }
    }

    stage('Test') {
      steps {
        echo "🧪 Running tests"
        sh 'docker compose run --rm notes-app python manage.py test || true'
      }
    }

    stage('Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-creds',
                                          usernameVariable: 'DOCKERHUB_USER',
                                          passwordVariable: 'DOCKERHUB_PASS')]) {
          sh """
            echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
            docker tag django-notes-app_notes-app ${DOCKER_REPO}:${BUILD_NUMBER}
            docker push ${DOCKER_REPO}:${BUILD_NUMBER}
            docker push ${DOCKER_REPO}:latest
            docker logout
          """
        }
      }
    }

    stage('Deploy') {
      steps {
        echo "🚀 Deploying"
        sh """
          docker compose down || true
          docker compose up -d
        """
      }
    }
  }
}
