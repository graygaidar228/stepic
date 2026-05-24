pipeline {
    agent any
    environment {
        PROJECT_NAME = 'CloudStore'
        DEPLOY_ENVIRONMENT = 'staging'
        RUN_SECURITY_SCAN = 'true'
    }
    stages {
        stage('Initialization') {
            steps {
                script {
                    echo "Starting ${PROJECT_NAME} Pipeline"
                    def services = ['auth-service', 'api-gateway', 'user-service', 'payment-service']
                    env.SERVICES = services.join(',')
                    echo "Services to build: ${env.SERVICES}"
                }
            }
        }
        stage('Build Services') {
            steps {
                script {
                    def services = env.SERVICES.split(',')
                    for (service in services) {
                        echo "Building ${service}..."
                        sh "mkdir build/${service}"
                        sh "touch build/${service}/app.jar"
                        sleep 1
                        echo "Build completed for ${service}"
                    }
                }
            }
        }
        stage('Unit Tests') {
            when {
                expression { env.BUILD_NUMBER % 2 != 0 }
            }
            steps {
                echo "Running unit tests for build ${env.BUILD_NUMBER}"
                sleep 2
                echo "Unit tests passed"
            }
        }
        stage('Integration Tests') {
            when {
                expression { env.BUILD_NUMBER % 2 == 0 }
            }
            steps {
                echo "Running integration tests for build ${env.BUILD_NUMBER}"
                sleep 2
                echo "Integration tests passed"
            }
        }
        stage('Security Scan') {
            when {
                expression { env.RUN_SECURITY_SCAN == 'true' }
            }
            steps {
                echo "Running security vulnerability scan..."
                sleep 3
                echo "Security scan completed - no vulnerabilities found"
            }
            post {
                always {
                    echo "Security scan stage finished"
                }
            }
        }
        stage('Deployment Approval') {
            when {
                expression { DEPLOY_ENVIRONMENT == 'production' || DEPLOY_ENVIRONMENT == 'staging' }
            }
            steps {
                script {
                    timeout(time: 5, unit: 'MINUTES') {
                        def userInput = input(
                            message: "Approve deployment to ${DEPLOY_ENVIRONMENT}?",
                            parameters: [
                                choice(
                                    name: 'DEPLOY_STRATEGY',
                                    choices: ['rolling', 
                                    'blue-green', 'canary']
                                    ),
                                booleanParam(
                                    name: 'SEND_NOTIFICATIONS', 
                                    defaultValue: true 
                                    )
                            ]
                        )
                        echo "Deploy: ${userInput.DEPLOY_STRATEGY}"
                        echo "Notifications: ${userInput.SEND_NOTIFICATIONS}"
                        env.DEPLOY_STRATEGY = userInput.DEPLOY_STRATEGY
                    }
                }
            }
        }
        stage('Deploy Services') {
            when {
                expression { DEPLOY_ENVIRONMENT == 'production' || DEPLOY_ENVIRONMENT == 'staging' }
            }
            steps {
                script {
                    def Maps = [
                        'staging': ['stage1.example.com', 'stage2.example.com'],
                        'production': ['prod1.example.com', 'prod2.example.com', 'prod3.example.com']
                    ]
                    def servers = Maps[DEPLOY_ENVIRONMENT]
                    def services = env.SERVICES.split(',')

                    servers.each { server ->
                        services.each { service ->
                            echo "Deploying ${service} to ${server} using ${env.DEPLOY_STRATEGY} strategy"
                            sleep 1
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            echo "=== Pipeline Execution Complete ==="
            echo "Total build time: ${currentBuild.durationString}"
        }
        success {
            echo "✓ Deployment SUCCESS"
            echo "Project: ${PROJECT_NAME}"
            echo "Environment: ${DEPLOY_ENVIRONMENT}"
            echo "All services deployed successfully"

            script {
                def reportContent = """
    Deployment Report
    ==================
    Project: ${PROJECT_NAME}
    Environment: ${DEPLOY_ENVIRONMENT}
    Build Number: ${env.BUILD_NUMBER}
    Build URL: ${env.BUILD_URL}
    Services: ${env.SERVICES}
    Deploy Strategy: ${env.DEPLOY_STRATEGY ?: 'N/A'}
    Status: SUCCESS
    Timestamp: ${new Date()}
    """
                writeFile file: 'deployment-report.txt', text: reportContent
            }
        }
        failure {
            echo "✗ Deployment FAILED"
            echo "Build Number: ${env.BUILD_NUMBER}"
            echo "Check logs at: ${env.BUILD_URL}"
            echo "Rolling back changes..."
        }
        cleanup {
            echo "Cleaning up temporary files..."
            echo "Cleanup completed"
        }
    }
}
