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
    }
}
