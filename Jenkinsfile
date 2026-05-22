pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Building application..."
                sh 'mkdir build'
                sh 'echo "Application binary" > build/app.jar'
            }
        }
        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'sleep 2'
                echo 'Tests completed'
            }
        }
    }
    post {
        always {
            echo "=== Post Actions ==="
            echo "Pipeline completed"
            sh 'date'
            }
        success {
            echo "✓ Build SUCCESS"
            echo "Build Number: ${env.BUILD_NUMBER}"
            echo "All stages passed successfully"
        }
        failure {
            echo "✗ Build FAILED"
            echo "Build Number: ${env.BUILD_NUMBER}"
            echo "Check console output for details"
            }
        }
    }
