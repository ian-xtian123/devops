pipeline {
  agent any
  stages {
    stage('SCM') {
      steps {
        git(url: 'https://github.com/ian-xtian123/company', branch: 'master')
      }
    }
  }
  environment {
    dotnet = 'C:\\\\Program Files\\\\dotnet\\\\dotnet.exe'
  }
}