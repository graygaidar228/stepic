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
    }
}
