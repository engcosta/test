pipeline {
  agent any
  stages {
    stage('pre-build') {
      steps {
        sh 'bat "dotnet restore test.csproj"'
        sh 'bat "dotnet clean test.csproj"'
      }
    }

  }
}