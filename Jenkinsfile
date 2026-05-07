pipeline {
    agent any

    environment {
        APP_NAME = "jenkins-demo"
        BUILD_ENV = "staging"
        BUILD_VERSION = "1.0.${env.BUILD_NUMBER}"
        SLACK_CHANNEL = "#jenkins-builds"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out code..."
                echo "Building version: ${env.BUILD_VERSION}"
                echo "Branch: ${env.GIT_BRANCH}"
            }
        }

        stage('Code Quality — SonarQube') {
            steps {
                echo "Running SonarQube analysis..."
                sh '''
                    echo "Analyzing code quality..."
                    sleep 1
                    echo "Code smells found: 0"
                    echo "Bugs found: 0"
                    echo "Vulnerabilities: 0"
                    echo "Coverage: 87%"
                    echo "Quality Gate: PASSED"
                '''
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

        stage('Publish Test Report') {
            steps {
                sh '''
                    mkdir -p test-results
                    cat > test-results/report.html << 'EOF'
<html>
<head><title>Test Report</title></head>
<body>
<h1>Test Report</h1>
<h2 style="color:green">All Tests Passed!</h2>
<table border="1" cellpadding="8">
  <tr><th>Test</th><th>Status</th><th>Duration</th></tr>
  <tr><td>Unit Test 1</td><td style="color:green">PASSED</td><td>0.3s</td></tr>
  <tr><td>Unit Test 2</td><td style="color:green">PASSED</td><td>0.2s</td></tr>
  <tr><td>Unit Test 3</td><td style="color:green">PASSED</td><td>0.4s</td></tr>
  <tr><td>Integration Test 1</td><td style="color:green">PASSED</td><td>0.8s</td></tr>
  <tr><td>Integration Test 2</td><td style="color:green">PASSED</td><td>0.6s</td></tr>
</table>
</body>
</html>
EOF
                    echo "Test report generated!"
                '''
                archiveArtifacts artifacts: 'test-results/report.html', fingerprint: true
            }
        }

        stage('Archive Artifacts') {
            steps {
                sh '''
                    mkdir -p artifacts
                    echo "App: ${APP_NAME}" > artifacts/build-info.txt
                    echo "Version: ${BUILD_VERSION}" >> artifacts/build-info.txt
                    echo "Branch: ${GIT_BRANCH}" >> artifacts/build-info.txt
                    echo "Build Date: $(date)" >> artifacts/build-info.txt
                    echo "Status: SUCCESS" >> artifacts/build-info.txt
                    cat artifacts/build-info.txt
                '''
                archiveArtifacts artifacts: 'artifacts/**', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'deploy-credentials',
                    usernameVariable: 'DEPLOY_USER',
                    passwordVariable: 'DEPLOY_PASS'
                )]) {
                    sh '''
                        echo "Deploying as user: $DEPLOY_USER"
                        echo "Password is hidden: ***"
                        echo "Deploying ${APP_NAME} to ${BUILD_ENV}..."
                        sleep 1
                        echo "Deploy complete!"
                    '''
                }
            }
        }

    }

    post {
        always {
            echo "Pipeline #${env.BUILD_NUMBER} finished"
            echo "SLACK: Notifying ${env.SLACK_CHANNEL}..."
        }
        success {
            echo "SUCCESS: ${env.APP_NAME} v${env.BUILD_VERSION} deployed to ${env.BUILD_ENV}!"
            echo "EMAIL: Notifying team of successful deployment..."
        }
        failure {
            echo "FAILURE: Build #${env.BUILD_NUMBER} failed — check logs!"
            echo "EMAIL: Alerting team of build failure..."
        }
    }
}