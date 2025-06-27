pipeline {
    agent any

    tools {
        dotnet 'dotnet6'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Branch') {
            when {
                anyOf {
                    branch 'main'
                    expression { env.BRANCH_NAME.startsWith('feature/') }
                }
            }
            steps {
                echo "Running pipeline for branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Restore') {
            when {
                anyOf {
                    branch 'main'
                    expression { env.BRANCH_NAME.startsWith('feature/') }
                }
            }
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            when {
                anyOf {
                    branch 'main'
                    expression { env.BRANCH_NAME.startsWith('feature/') }
                }
            }
            steps {
                sh 'dotnet build --configuration Release --no-restore'
            }
        }

        stage('Test') {
            when {
                anyOf {
                    branch 'main'
                    expression { env.BRANCH_NAME.startsWith('feature/') }
                }
            }
            steps {
                sh 'dotnet test --configuration Release --no-build --verbosity normal'
            }
        }
    }

    post {
        always {
            echo "Pipeline finished for ${env.BRANCH_NAME}"
        }
        failure {
            echo "Pipeline failed for ${env.BRANCH_NAME}"
        }
    }
}
