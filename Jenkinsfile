pipeline {
  agent any
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

  }
  environment {
    DOTNET_CORE_SDK_VERSION = '8.0'
  }
  post {
    always {
      echo 'Cleaning up...'
      cleanWs()
    }

  }
}