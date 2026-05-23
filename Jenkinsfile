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
                sh 'exit 1'
                echo 'Tests completed'
                sh 'exit 1'
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying application..."
                sh 'sleep 3'
                echo 'Deployment completed'
            }
            post {
                always {
                    echo 'Deploy stage finished'
                    sh 'ls -la build/'
                }
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
            echo "Archiving build artifacts..."
            sh 'tar -czf build.tar.gz build/'
            sh 'ls -lh build.tar.gz'
            echo 'Artifacts archived successfully'
        }
        failure {
            echo "✗ Build FAILED"
            echo "Build Number: ${env.BUILD_NUMBER}"
            echo "Check console output for details"
            }
        cleanup  {
            echo "=== Cleanup Phase ==="
            echo "Removing temporary files..."
            sh "mkdir temp && rm -rf temp"
            echo "Cleanup completed"
            }
        }
    }
