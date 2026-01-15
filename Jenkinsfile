pipeline {
    agent { label 'multipass' }

    options {
        timestamps()
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
                  echo "Branch: $(git rev-parse --abbrev-ref HEAD)"
                '''
            }
        }

        stage('Lint (All Branches)') {
            steps {
                sh '''
                  echo "Running flake8 lint..."
                  flake8 app/backend
                '''
            }
        }

        stage('Extra Checks (main only)') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                  echo "Running stricter checks for main branch"
                  sleep 2
                '''
            }
        }
    }

    post {
        success {
            echo 'CI PASSED'
        }
        failure {
            echo 'CI FAILED — fix issues before merging'
        }
    }
}
