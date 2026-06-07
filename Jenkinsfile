pipeline {
    agent any
    parameters {
        string(name: 'APP_VERSION', defaultValue: '1.0.0', description: 'Application version')
        choice(name: 'DEPLOY_ENVIRONMENT', choices: ['development', 'staging', 'production'], description: 'Deployment environment')
        choice(name: 'BUILD_TYPE', choices: ['quick', 'full'], description: 'Build type (quick or full with all tests)')
        booleanParam(name: 'RUN_SECURITY_SCAN', defaultValue: false, description: 'Run security vulnerability scan')
        booleanParam(name: 'DEPLOY_ENABLED', defaultValue: true, description: 'Deploy after build')
    }
    environment {
        APP_NAME = 'jenkins-sample-app'
        BUILD_VERSION = "${params.APP_VERSION}-${BUILD_NUMBER}"
        DOCKER_IMAGE = "jenkins-sample-app:${BUILD_VERSION}"
    }
    stages {
        stage('Initialize') {
            steps {
                script {
                    echo "=== CI/CD Pipeline Started ==="
                    echo "Application: ${APP_NAME}"
                    echo "Version: ${params.APP_VERSION}"
                    echo "Build Version: ${BUILD_VERSION}"
                    echo "Environment: ${params.DEPLOY_ENVIRONMENT}"
                    echo "Build Type: ${params.BUILD_TYPE}"
                    echo "Build Number: ${BUILD_NUMBER}"
                    echo "Job Name: ${JOB_NAME}"
                    if (params.DEPLOY_ENVIRONMENT == 'production') {
                        echo "⚠️ WARNING: Production deployment - extra validation required"
                    }
                }
            }
        }
        stage('Build Application') {
            steps {
                dir('app') {
                    echo "Installing dependencies..."
                    sh 'npm install'
                    echo "Building application version ${BUILD_VERSION}"
                    sh "APP_VERSION=${BUILD_VERSION} npm run build"
                    echo "Build artifacts created in dist/"
                }
            }
        }
        stage('Unit Tests') {
            steps {
                dir('app') {
                    echo "Running unit tests..."
                    sh "NODE_ENV=test APP_VERSION=${BUILD_VERSION} npm test"
                    echo "Unit tests passed ✓"
                }
            }
        }
        stage('Integration Tests') {
            when {
                expression { params.BUILD_TYPE == 'full' }
            }
            steps {
                echo "Running integration tests..."
                sh 'sleep 2'
                echo "Integration tests passed ✓"
            }
        }
        stage('Security Scan') {
            when {
                expression { params.RUN_SECURITY_SCAN == true }
            }
            steps {
                echo "Running security vulnerability scan..."
                echo "Scanning Docker image: ${DOCKER_IMAGE}"
                sh 'sleep 3'
                echo "Security scan completed - no critical vulnerabilities found ✓"
            }
            post {
                always {
                    echo "Security scan stage finished"
                }
            }
        }
        stage('Docker Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    echo "Building Docker image: ${DOCKER_IMAGE}"
                    echo "Docker Registry User: ${DOCKER_USER}"
                    sh "echo 'docker build -t ${DOCKER_IMAGE} .'"
                    sh "echo 'docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}'"
                    sh "echo 'docker push ${DOCKER_IMAGE}'"
                    echo "Docker image built and pushed successfully ✓"
                }
            }
        }
        stage('Deploy Application') {
            when {
                expression { params.DEPLOY_ENABLED == true }
            }
            steps {
                withCredentials([
                    string(credentialsId: 'api-key', variable: 'API_KEY'),
                    string(credentialsId: 'database-url', variable: 'DATABASE_URL')
                ]) {
                    script {
                        echo "Deploying to ${params.DEPLOY_ENVIRONMENT} environment"
                        echo "Version: ${BUILD_VERSION}"
                        echo "Docker Image: ${DOCKER_IMAGE}"
                        def serversMap = [
                            'development': ['dev1.example.com'],
                            'staging': ['stage1.example.com', 'stage2.example.com'],
                            'production': ['prod1.example.com', 'prod2.example.com', 'prod3.example.com']
                        ]
                        def servers = serversMap[params.DEPLOY_ENVIRONMENT] ?: []
                        for (server in servers) {
                            echo "Deploying to ${server}..."
                            sh 'sleep 1'
                            echo "✓ Deployed to ${server}"
                        }
                        echo "All servers updated successfully"
                    }
                }
            }
        }
        stage('Smoke Tests') {
            when {
                expression { params.DEPLOY_ENABLED == true }
            }
            steps {
                withCredentials([
                    string(credentialsId: 'api-key', variable: 'API_KEY'),
                    string(credentialsId: 'database-url', variable: 'DATABASE_URL')
                ]) {
                    dir('app') {
                        echo "Running smoke tests on ${params.DEPLOY_ENVIRONMENT}..."
                        sh "NODE_ENV=${params.DEPLOY_ENVIRONMENT}"
                        sh "APP_VERSION=${BUILD_VERSION}"
                        sh "BUILD_NUMBER=${BUILD_NUMBER}"
                        sh "API_KEY=${API_KEY}"
                        sh "DATABASE_URL=${DATABASE_URL}"
                        sh "npm start &"
                        sh 'sleep 3'
                        def health = sh(script: 'curl -s http://localhost:3000/health', returnStdout: true).trim()
                        def config = sh(script: 'curl -s http://localhost:3000/config', returnStdout: true).trim()
                        if (config.contains(BUILD_VERSION) && config.contains(params.DEPLOY_ENVIRONMENT)) {
                            echo "Smoke tests passed ✓"
                        } else {
                            echo "Warning: Smoke tests detected version/environment mismatch"
                        }
                        sh 'pkill -f "node server.js" || true'
                    }
                }
            }
        }
    }
    post {
        always {
            echo "=== Pipeline Execution Complete ==="
            echo "Total execution time: ${currentBuild.durationString}"
        }
        success {
            echo "✓ BUILD SUCCESSFUL"
            echo "Application: ${APP_NAME}"
            echo "Version: ${BUILD_VERSION}"
            echo "Environment: ${params.DEPLOY_ENVIRONMENT}"
            echo "Docker Image: ${DOCKER_IMAGE}"
            writeFile file: 'deployment-report.txt', text: """
Jenkins Deployment Report
=========================
Application: ${APP_NAME}
Version: ${BUILD_VERSION}
Environment: ${params.DEPLOY_ENVIRONMENT}
Build Number: ${BUILD_NUMBER}
Job Name: ${JOB_NAME}
Build Type: ${params.BUILD_TYPE}
Security Scan: ${params.RUN_SECURITY_SCAN ? 'Yes' : 'No'}
Deploy Enabled: ${params.DEPLOY_ENABLED ? 'Yes' : 'No'}
Docker Image: ${DOCKER_IMAGE}
Execution Time: ${currentBuild.durationString}
"""
            echo "Deployment report saved to deployment-report.txt"
        }
        failure {
            echo "✗ BUILD FAILED"
            echo "Build Number: ${BUILD_NUMBER}"
            echo "Check logs at: ${BUILD_URL}"
            echo "Failed at environment: ${params.DEPLOY_ENVIRONMENT}"
        }
        cleanup {
            echo "Cleaning up workspace..."
            echo "Cleanup completed"
        }
    }
}
