pipeline {
    agent any
    environment {
        APP_VERSION = '1.0.0'
        ENVIRONMENT = 'development'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    dir('python-app'){
                        sh "python3 -m venv venv && source venv/bin/activate && pip3 install -r requirements.txt"
                        sh "APP_VERSION=$APP_VERSION BUILD_NUMBER=$BUILD_NUMBER ENVIRONMENT=$ENVIRONMENT python3 build.py"

                    }
                }
            }
        }
        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: 'python-app/dist/build-info.json, python-app/dist/BUILD-REPORT.txt'
            }
        }
    }
}

