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
                  echo "=== LINT STAGE ==="

                  echo "Ensuring Python is available"
                  python3 --version

                  echo "Creating virtual environment for CI"
                  python3 -m venv .ci-venv

                  echo "Activating virtual environment"
                  . .ci-venv/bin/activate

                  echo "Upgrading pip"
                  pip install --upgrade pip

                  echo "Installing flake8 inside venv"
                  pip install flake8

                  echo "Running flake8"
                  flake8 app/backend

                  echo "Deactivating venv"
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
