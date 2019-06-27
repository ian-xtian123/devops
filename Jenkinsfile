pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat(script: 'nuget restore "Case Study.sln" -source "https://api.nuget.org/v3/index.json"', label: 'Restore Packages', returnStatus: true)
      }
    }
  }
}