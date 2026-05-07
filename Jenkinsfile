pipeline {
    agent any

    environment {
        APP_NAME = "jenkins-demo"
        BUILD_ENV = "staging"
        BUILD_VERSION = "1.0.${env.BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out code..."
                echo "Building version: ${env.BUILD_VERSION}"
                echo "Branch: ${env.GIT_BRANCH}"
            }
        }

        stage('Build') {
            steps {
                echo "Building ${env.APP_NAME} v${env.BUILD_VERSION}"
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

        stage('Test') {
            parallel {

                stage('Unit Tests') {
                    steps {
                        sh '''
                            echo "Running unit tests..."
                            sleep 1
                            echo "Unit tests: 3/3 passed"
                        '''
                    }
                }

                stage('Integration Tests') {
                    steps {
                        sh '''
                            echo "Running integration tests..."
                            sleep 1
                            echo "Integration tests: 2/2 passed"
                        '''
                    }
                }

                stage('Security Scan') {
                    steps {
                        sh '''
                            echo "Scanning for vulnerabilities..."
                            sleep 1
                            echo "Security scan: Clean"
                        '''
                    }
                }

            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying ${env.APP_NAME} v${env.BUILD_VERSION} to ${env.BUILD_ENV}"
                sh '''
                    echo "Deploy started..."
                    sleep 1
                    echo "Deploy complete!"
                '''
            }
        }

    }

    post {
        always {
            echo "Pipeline #${env.BUILD_NUMBER} finished"
        }
        success {
            echo "SUCCESS: ${env.APP_NAME} v${env.BUILD_VERSION} deployed!"
        }
        failure {
            echo "FAILURE: Build #${env.BUILD_NUMBER} failed — check logs!"
        }
    }
}