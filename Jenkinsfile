pipeline {
    agent any

    environment {
        // Define environment variables if needed
        DOTNET_CORE_SDK_VERSION = '8.0'
        PATH = "${tool 'dotnet'}:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Get the latest code from the GitHub repository
                git 'https://github.com/engcosta/test.git'
            }
        }

        stage('Restore') {
            steps {
                // Restore the .NET Core packages
                bat 'dotnet restore test.sln'
            }
        }

        stage('Build') {
            steps {
                // Build the .NET Core project
                bat 'dotnet build test.sln --configuration Release'
            }
        }

        stage('Test') {
            steps {
                // Run tests here if you have any test projects
                // bat 'dotnet test YourTestProject.csproj --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                // Publish the Web API project
                bat 'dotnet publish test.csproj --configuration Release --output ./publish'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy to IIS using Web Deploy or any other method you prefer
                // Make sure to replace 'YourIISAppName' with your actual IIS app name
                // and provide the correct credentials and server details
                bat """
                msdeploy.exe -verb:sync -source:contentPath='./publish' -dest:contentPath='test',ComputerName='https://localhost:8000/msdeploy.axd'
                """
            }
        }
    }

    post {
        always {
            // Clean up after the pipeline runs
            cleanWs()
        }
    }
}
