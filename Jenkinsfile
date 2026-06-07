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
                        sh "python3 -m venv venv && . venv/bin/activate && pip3 install -r requirements.txt"
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
        stage('Archive All Build Output') {
            steps {
                archiveArtifacts artifacts: 'python-app/dist/**/*'
            }
        }
        stage('Test') {
            steps {
                dir('python-app') {
                    sh ". venv/bin/activate && ENVIRONMENT=test APP_VERSION=${APP_VERSION} pytest -v --cov=app --cov-report=html --cov-report=xml --junit-xml=test-results.xml"
                    echo "Tests completed"
                }
            }
        }
        stage('Archive Test Reports') {
            steps {
                archiveArtifacts artifacts: 'python-app/test-results.xml, python-app/coverage.xml, python-app/htmlcov/**/*'
            }
        }
        stage('Archive Multiple Types') {
            steps {
                archiveArtifacts artifacts: 'python-app/dist/package/*, python-app/dist/docs/*.md, python-app/*.xml'
            }
        }
        stage('Archive Optional Files') {
            steps {
                archiveArtifacts artifacts: 'python-app/*.log', allowEmptyArchive: true
            }
        }
        stage('Archive Package with Fingerprint') {
            steps {
                archiveArtifacts artifacts: 'python-app/dist/package/**', fingerprint: true
            }
        }
    }
}
