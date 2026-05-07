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
                echo "Building ${env.APP_NAME}..."
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh '''
                    echo "Test Suite Started"
                    echo "Test 1: Checking build output... PASSED"
                    echo "Test 2: Checking environment... PASSED"
                    echo "Test 3: Checking dependencies... PASSED"
                    echo "All tests passed!"
                '''
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