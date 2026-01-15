pipeline {
    agent { label 'multipass' }

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Branch Info') {
            steps {
                sh '''
                  echo "Git version:"
                  git --version

                  echo "Branch (detached HEAD in CI is normal):"
                  git rev-parse --abbrev-ref HEAD || true

                  echo "Commit SHA:"
                  git rev-parse --short HEAD
                '''
            }
        }

        stage('Lint (All Branches)') {
            steps {
                sh '''
                  set -e

                  echo "=== LINT STAGE ==="

                  echo "Python version:"
                  python3 --version

                  echo "Ensuring venv support is available"
                  sudo apt-get update
                  sudo apt-get install -y python3-venv

                  echo "Creating CI virtual environment"
                  python3 -m venv .ci-venv

                  echo "Activating virtual environment"
                  . .ci-venv/bin/activate

                  echo "Upgrading pip"
                  pip install --upgrade pip

                  echo "Installing flake8"
                  pip install flake8

                  echo "Running flake8"
                  flake8 app/backend

                  echo "Deactivating virtual environment"
                  deactivate
                '''
            }
        }

        stage('Extra Checks (main only)') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                  echo "=== MAIN BRANCH EXTRA CHECKS ==="
                  echo "Additional validations for main branch"
                  sleep 2
                '''
            }
        }
    }

    post {
        success {
            echo '✅ CI PASSED'
        }
        failure {
            echo '❌ CI FAILED — fix issues before merging'
        }
        always {
            sh 'rm -rf .ci-venv || true'
        }
    }
}
