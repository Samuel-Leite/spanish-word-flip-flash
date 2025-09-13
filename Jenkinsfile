pipeline {
    agent any

    options {
        ansiColor('xterm')
    }

    environment {
        NODE_ENV = 'test'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            agent {
                docker {
                    image 'node:22-alpine'
                }
            }
            steps {
                sh 'npm ci'
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:22-alpine'
                }
            }
            steps {
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

                stage('Integration Tests') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.54.2-jammy'
                        }
                    }
                    steps {
                        sh 'npx playwright install'
                        sh 'npx playwright test'
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