pipeline {
    agent any
    stages {
        stage('Variables Demo') {
            steps{
                script {
                def appName = "MyApplication"
                def port = 8080
                def isProduction = true
                echo "${appName}"
                echo "${port}"
                echo "${isProduction}"
                }
            }
        }
        stage('String Operations') {
            steps{
                script {
                def message = "Jenkins Pipeline Tutorial"
                echo "${message.length()}"
                echo "${message.toUpperCase()}"
                echo "${message.toLowerCase()}"
                def new_message = message.replace('Tutorial','Course')
                echo "${new_message}"
                }
            }
        }
        stage('Build Version') {
            steps{
                script {
                def major = 1
                def minor = 0
                def patch = env.BUILD_NUMBER
                env.APP_VERSION = "${major}.${minor}.${patch}"
                echo "Application version: ${env.APP_VERSION}"
                }
            }
        }
        stage('Display Version') {
            steps{
                script {
                echo "Using version: ${env.APP_VERSION}"
                def imageName = "myapp:1.0.${env.APP_VERSION}"
                echo "Docker image would be: ${imageName}"
                }
            }
        }
        stage('Jenkins_Info') {
            steps {
                script {
                    echo "Build Number: ${env.BUILD_NUMBER}"
                    echo "Build ID: ${env.BUILD_ID}"
                    echo "Job Name: ${env.JOB_NAME}"
                    echo "Workspace: ${env.WORKSPACE}"
                    echo "Build URL: ${env.BUILD_URL}"
                }
            }
        }
    }
}
