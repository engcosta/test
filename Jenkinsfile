pipeline {
    agent any

    environment {
        // Define environment variables if needed
        DOTNET_CORE_SDK_VERSION = '3.1'
    }

    stages {
        stage('Restore') {
            steps {
                echo 'Restoring dependencies...'
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Replace 'test' with your project's name
                bat 'dotnet build test.csproj --configuration Release'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'dotnet test test.csproj --configuration Release --logger "trx;LogFileName=test_results.trx"'
            }
        }

        stage('Publish Test Results') {
            steps {
                echo 'Publishing test results...'
                junit '**/test_results.trx'
            }
        }

        // Add additional stages for deployment if necessary
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs() // Cleans up the workspace
        }
    }
}
