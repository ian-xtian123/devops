pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat(script: 'nuget restore "Case Study.sln"', label: 'Restore Packages', returnStatus: true)
      }
    }
  }
}