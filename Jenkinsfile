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
                  echo "Git info:"
                  git --version
                  echo "Current branch:"
                  git rev-parse --abbrev-ref HEAD || true
                  echo "Commit SHA:"
                  git rev-parse --short HEAD
                '''
            }
        }

        stage('Lint (All Branches)') {
            steps {
                sh '''
                  echo "=== Lint Stage ==="
                  echo "Ensuring Python & pip are available"
                  python3 --version
                  pip3 --version || sudo apt-get update && sudo apt-get install -y python3-pip

                  echo "Installing flake8 locally for CI user"
                  pip3 install --user flake8

                  echo "Running flake8"
                  ~/.local/bin/flake8 app/backend
                '''
            }
        }

        stage('Extra Checks (main only)') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                  echo "=== Main Branch Extra Checks ==="
                  echo "Running additional validations for main"
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
    }
}
