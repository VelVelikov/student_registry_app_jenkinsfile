pipeline {
    agent any

    environment {
        NODE_VERSION = 'node20'
    }

    tools {
        nodejs "${NODE_VERSION}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Start Application') {
            steps {
                // Run app in background
                sh 'nohup npm start &'
                sleep 5 // Give it time to start
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
