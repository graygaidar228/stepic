pipeline {
    agent any
    stages {
        stage('Выбор пакетов') {
            steps {
                script {
                    def packageIds = ['pkg-001', 'pkg-002', 'pkg-003', 'pkg-004']

                    def selectedPackages = input(
                        message: 'Выберите Package ID для сборки:',
                        parameters: [
                            [
                                $class: 'ChoiceParameterDefinition',
                                name: 'PACKAGES',
                                choices: packageIds,
                                description: 'Доступные Package ID',
                            ]
                        ]
                    )
                }
            }
        }
    }
}
