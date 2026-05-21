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
                    echo "$(environments.add('qa'))"
                    echo "${environments.size()}"
                }
            }
        }
    }
}
