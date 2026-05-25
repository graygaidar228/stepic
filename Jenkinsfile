pipeline {
    agent any
    environment{
        APP_NAME = 'jenkins-sample-app'
        NODE_ENV = 'development'
        PORT = '3000'
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
                sh "mkdir -p app"
                sh "npm install"
                echo "Dependencies installed for ${APP_NAME}"
                }
            }
        }
    }
