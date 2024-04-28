pipeline {
  agent any
  stages {
    stage('Restore') {
      steps {
        echo 'Restoring dependencies...'
        script {
          if (isUnix()) {
            sh 'export PATH=$PATH:/usr/local/share/dotnet && dotnet restore'
          } else {
            bat 'dotnet restore'
          }
        }

      }
    }

    stage('Build') {
      steps {
        echo 'Building the project...'
        script {
          if (isUnix()) {
            sh 'export PATH=$PATH:/usr/local/share/dotnet && dotnet build test.csproj --configuration Release'
          } else {
            bat 'dotnet build test.csproj --configuration Release'
          }
        }

      }
    }

    stage('Test') {
      steps {
        echo 'Running tests...'
        script {
          if (isUnix()) {
            sh 'export PATH=$PATH:/usr/local/share/dotnet && dotnet test test.csproj --configuration Release --logger "trx;LogFileName=test_results.trx"'
          } else {
            bat 'dotnet test test.csproj --configuration Release --logger "trx;LogFileName=test_results.trx"'
          }
        }

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
    PATH = "${env.PATH}:/usr/local/share/dotnet"
  }
  post {
    always {
      echo 'Cleaning up...'
      cleanWs()
    }

  }
}