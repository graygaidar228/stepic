pipeline {
    agent any
    stages {
        stage('List Basics') {
            steps{
                script{
                    def environments = ['dev', 'staging', 'production']
                    echo "${environments[0]}"
                    echo "${environments[-1]}"
                    echo "${environments.size()}"
                    environments.add('qa')
                    echo "${environments.size()}"
                }
            }
        }
        stage('Deploy to Servers') {
            steps{
                script{
                    def servers =  ['server1.example.com', 'server2.example.com', 'server3.example.com']
                    for (server in servers){
                        echo "Deploying to ${server}"
                        sleep 1 
                        echo "{Deployment to server completed}"
                    }
                }
            }
        }
        stage('Configuration Map') {
            steps{
                script{
                    def dict =  [
                        appName: 'MyWebApp',
                        version: '2.0.0',
                        port: '8080',
                        environment: 'production'
                    ]
                    dict.each { key, value ->
                        echo "${value}"
                    }
                    echo "${dict.size()}"
                    dict['region'] = "us-east-1"
                    dict.each { key, value ->
                        echo "${key}: ${value}"
                    }
                }
            }
        }
        stage('Environment Variables') {
            steps{
                script{
                    def dict =  [
                        DATABASE_URL: 'postgresql://db.example.com:5432/mydb',
                        CACHE_URL: 'redis://cache.example.com:6379',
                        LOG_LEVEL: 'info',
                    ]
                    dict.each { key, value ->
                        echo "${key}: ${value}"
                    }
                }
            }
        }
        stage('Multi-Environment Deploy') {
            steps{
                script{
                    def deployments = [
                        'dev': ['dev1.example.com', 'dev2.example.com'],
                        'staging': ['stage1.example.com'],
                        'prod': ['prod1.example.com', 'prod2.example.com', 'prod3.example.com']
                    ]
                    deployments.each { key, value ->
                        for (serv in value){
                            echo "Deploying to ${key}: ${serv}"
                        }
                    }
                }
            }
        }
    }
}
