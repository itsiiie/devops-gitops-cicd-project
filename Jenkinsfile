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

        stage('CI Info') {
            steps {
                sh '''
                  echo "Branch: $(git rev-parse --abbrev-ref HEAD)"
                  echo "Commit: $(git rev-parse --short HEAD)"
                '''
            }
        }

        stage('Sanity Checks') {
            steps {
                sh '''
                  echo "Running sanity checks..."
                  hostname
                  whoami
                '''
            }
        }
    }

    post {
        success {
            echo 'CI pipeline completed successfully'
        }
        failure {
            echo 'CI pipeline failed'
        }
    }
}
