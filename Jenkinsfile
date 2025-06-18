pipeline {
    agent any

    // environment {
    //     NODE_VERSION = 'node20'
    // }

    // tools {
    //     nodejs "${NODE_VERSION}"
    // }

    environment {
        DEPLOY_HOOK = credentials('RENDER_DEPLOY_HOOK')
    }

    stages {
        stage('Checkout the repository') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Audit and Test Parallel'){
            parallel {
                stage('Security audit') {
                    steps {
                        sh 'npm audit'
                    }
                }

                stage('Run Tests') {
                    steps {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Confirm Deploy') {
            steps {
                script {
                input message: 'Do you want to deploy to production?', ok: 'Deploy'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'curl -X POST $DEPLOY_HOOK'
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
