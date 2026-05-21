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
    }
}
