pipeline {
  agent any
  stages {
    stage ('Clean workspace') {
      steps {
        cleanWs()
      }
    }
    stage('Restore packages') {
      steps {
        bat "dotnet restore ${workspace}\\test.sln"
      }
    }
    stage('Clean') {
      steps {
        bat "msbuild.exe ${workspace}\\test.sln" /nologo /nr:false /p:platform=\"x64\" /p:configuration=\"release\" /t:clean"
      }
    }
    stage('Increase version') {
      steps {
          echo "${env.BUILD_NUMBER}"
          powershell '''
            $xmlFileName = "test\\Package.appxmanifest"
            [xml]$xmlDoc = Get-Content $xmlFileName
            $version = $xmlDoc.Package.Identity.Version
            $trimmedVersion = $version -replace '.[0-9]+$', '.'
            $xmlDoc.Package.Identity.Version = $trimmedVersion + ${env:BUILD_NUMBER}
            echo 'New version:' $xmlDoc.Package.Identity.Version
            $xmlDoc.Save($xmlFileName)
          '''
      }
    }
  stage('Build') {
    steps {
      bat "msbuild.exe ${workspace}\\test.sln /nologo /nr:false  /p:platform=\"x64\" /p:configuration=\"release\" /p:PackageCertificateKeyFile=<path-to-certificate-file>.pfx /t:clean;restore;rebuild"
    }
  }

  }
}
