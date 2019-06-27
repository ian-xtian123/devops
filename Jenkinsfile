pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat(script: 'nuget restore "Case Study.sln" -source "https://api.nuget.org/v3/index.json"', label: 'Restore Packages', returnStatus: true)
        bat(script: '"${tool \'MSBuild\'}"  SolutionName.sln /p:Configuration=Release /p:Platform="Any CPU" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}', label: 'MS Build', returnStatus: true)
      }
    }
  }
}
