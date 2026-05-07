pipeline {
    agent any

    environment {
        APP_NAME = "jenkins-demo"
        BUILD_ENV = "staging"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                echo "Branch: ${env.GIT_BRANCH}"
                echo "Commit: ${env.GIT_COMMIT}"
            }
        }

        stage('Build') {
            steps {
                echo "Building..."
                sh './build.sh'
                sh 'exit 1'    // force a failure
            }
        }

        stage('Test') {
            parallel {

                stage('Unit Tests') {
                    steps {
                        echo "Running unit tests..."
                        sh '''
                            sleep 1
                            echo "Unit Test 1: PASSED"
                            echo "Unit Test 2: PASSED"
                            echo "Unit Test 3: PASSED"
                        '''
                    }
                }

                stage('Integration Tests') {
                    steps {
                        echo "Running integration tests..."
                        sh '''
                            sleep 1
                            echo "Integration Test 1: PASSED"
                            echo "Integration Test 2: PASSED"
                        '''
                    }
                }

                stage('Security Scan') {
                    steps {
                        echo "Running security scan..."
                        sh '''
                            sleep 1
                            echo "No vulnerabilities found"
                        '''
                    }
                }

            }
        }

        stage('Deploy') {
                steps {
                    echo "Deploying to ${env.BUILD_ENV}..."
                    sh '''
                        echo "Copying files to staging server..."
                        echo "Restarting services..."
                        echo "Deploy complete!"
                    '''
                }
            }

    }

    post {
        always {
            echo "Pipeline finished — cleaning up workspace"
        }
        success {
            echo "BUILD SUCCEEDED! App deployed to ${env.BUILD_ENV}"
        }
        failure {
            echo "BUILD FAILED! Check console output for errors"
        }
    }
}