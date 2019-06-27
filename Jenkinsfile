pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarQube') {
          bat "\"${tool 'MSBuildSQ'}\"  begin /k:\"caseStudy\" /d:sonar.host.url=\"http://localhost:9000\" /d:sonar.login=\"b0730da2e5486dde54f6acb79e6740adee3615a8\""
          bat(script: 'nuget restore "Case Study.sln" -source "https://api.nuget.org/v3/index.json"', label: 'Restore Packages', returnStatus: true)
          bat "\"${tool 'MSBuild'}\" \"Case Study.sln\" /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}"
          bat "\"${tool 'MSBuildSQ'}\" end"
        }

      }
    }
    stage('Archive') {
      steps {
        zip(archive: true, zipFile: "caseStudy.1.0.0.${env.BUILD_NUMBER}.zip", dir: 'Case Study/bin/')
        archiveArtifacts "caseStudy.1.0.0.${env.BUILD_NUMBER}.zip"
      }
    }
  }
}