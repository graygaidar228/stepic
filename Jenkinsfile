pipeline {
    agent any
    environment{
        APP_NAME = 'jenkins-sample-app'
        NODE_ENV = 'development'
        PORT = '3000'
        APP_VERSION = "1.0.{BUILD_NUMBER}"
    }
    stages {
        stage('Show Build Info') {
            steps {
                echo "Build Number: ${BUILD_NUMBER}"
                echo "Job Name: ${JOB_NAME}"
                echo "Workspace: ${WORKSPACE}"
                echo "Build URL: ${BUILD_URL}"
                }
            }
        stage('Install Dependencies') {
            steps {
                sh "cd app"
                sh "cd app && npm install"
                echo "Dependencies installed for ${APP_NAME}"
                }
            }
        stage('Build') {
            steps {
                sh "cd app"
                echo "Building ${APP_NAME}: ${APP_VERSION}"
                sh "cd app && npm run build"
                echo "Build completed successfully"
                }
            }
        stage('Test ') {
            environment{
                NODE_ENV = 'test'
            }
            steps {
                sh "cd app"
                echo "Running tests in ${NODE_ENV} environment"
                echo "Building ${APP_NAME}: ${APP_VERSION}"
                sh "cd app && NODE_ENV=$NODE_ENV APP_VERSION=$APP_VERSION npm test"
                }
            }
        stage('Run Application') {
            steps {
                sh "cd app"
                echo "Starting ${APP_NAME} on port ${PORT}"
                sh "cd app && NODE_ENV=$NODE_ENV APP_VERSION=$APP_VERSION BUILD_NUMBER=$BUILD_NUMBER PORT=$PORT npm start &"
                sh "sleep 3"
                sh "curl http://localhost:${PORT}/"
                sh "curl http://localhost:${PORT}/config"
                sh 'pkill -f "node server.js"'
                }
            }

        }
    }
