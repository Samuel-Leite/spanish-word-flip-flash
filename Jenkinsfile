pipeline {
    agent any

    options {
        ansiColor('xterm')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:22-alpine'
                }
            }
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }

        stage('Test') {
            parallel {
                stage('Unit Tests') {
                    agent {
                        docker {
                            image 'node:22-alpine'
                        }
                    }
                    steps {
                        sh 'npx vitest run --reporter=verbose'
                    }
                }
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'alpine'
                }
            }
            steps {
                echo 'Mock deployment was successful!'
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
    }
}