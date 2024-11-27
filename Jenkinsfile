pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo "Building the repository"
      }
    }

    stage('Test') {
      steps {
        echo "Running tests..."
        sh 'python3 test_app.py'
      }
    }

    stage('Deploy') {
      when {
        input {
          message "Do you want to deploy the application?"
          ok "Deploy"
        }
      }
      steps {
        echo "Deploying the application"
      }
    }
  }

  post {
    always {
      echo 'The pipeline has completed'
      junit allowEmptyResults: true, testResults: '**/test_reports/*.xml'
    }
    success {
      sh '''
        nohup python3 app.py > log.txt 2>&1 &
      '''
      echo "Flask application is up and running!"
    }
    failure {
      echo 'The pipeline failed during execution'
    }
  }
}
