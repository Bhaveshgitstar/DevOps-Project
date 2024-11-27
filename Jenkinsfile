pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh 'echo "building the repo"'
          }
        }
      }
    }

    stage('Test') {
            steps {
                sh '''
                if ! command -v python3 &> /dev/null; then
                    echo "Python3 not found. Installing Python3..."
                    sudo apt-get update
                    sudo apt-get install -y python3 python3-pip
                fi
                python3 test_app.py
                '''
                input(id: "Deploy Gate", message: "Deploy the application?", ok: 'Deploy')
            }
    }

    stage('Deploy')
    {
      steps {
        echo "deploying the application"
      }
    }

  }

  post {
        always {
            echo 'The pipeline completed'
            junit allowEmptyResults: true, testResults:'**/test_reports/*.xml'
        }
        success {
            
            sh "sudo nohup python3 app.py > log.txt 2>&1 &"
            echo "Flask Application Up and running!!"
        }
        failure {
            echo 'Build stage failed'
            error('Stopping early…')
        }
      }
}
