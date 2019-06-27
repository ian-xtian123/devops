pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat(script: 'nuget restore "Case Study.sln" -source "https://api.nuget.org/v3/index.json"', label: 'Restore Packages', returnStatus: true)
        bat "\"${tool 'MSBuild'}\" \"Case Study.sln\" /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}"
      }
    }
    stage('Archive') {
      steps {
        zip(archive: true, zipFile: 'caseStudy.1.0.0."${env.BUILD_NUMBER}".zip', dir: 'Case Study/bin/')
        archiveArtifacts 'caseStudy.1.0.0."${env.BUILD_NUMBER}".zip'
      }
    }
  }
}