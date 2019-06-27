pipeline {
  agent any
  stages {
    stage('Build, Test and Code Quality') {
      steps {
        withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarQube') {
          bat "\"${tool 'MSBuildSQ'}\"  begin /k:\"caseStudy\" /d:sonar.host.url=\"http://localhost:9000\" /d:sonar.login=\"b0730da2e5486dde54f6acb79e6740adee3615a8\" /v:'caseStudy.1.0.0.${env.BUILD_NUMBER}' /d:sonar.cs.vstest.reportsPaths='TestResults\\*.trx'"
          bat(script: 'nuget restore "Case Study.sln" -source "https://api.nuget.org/v3/index.json"', label: 'Restore Packages', returnStatus: true)
          bat "\"${tool 'MSBuild'}\" \"Case Study.sln\" /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}"
          bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\Common7\\IDE\\Extensions\\TestPlatform\\vstest.console.exe" /Logger:trx "CaseStudy.Tests\\bin\\Release\\CaseStudy.Tests.dll"'
          bat "\"${tool 'MSBuildSQ'}\" end /d:sonar.login=\"b0730da2e5486dde54f6acb79e6740adee3615a8\""
        }

        waitForQualityGate(credentialsId: 'SonarQube', abortPipeline: true)
      }
    }
    stage('Archive') {
      steps {
        zip(archive: true, zipFile: "caseStudy.1.0.0.${env.BUILD_NUMBER}.zip", dir: 'Case Study/bin/')
        archiveArtifacts "caseStudy.1.0.0.${env.BUILD_NUMBER}.zip"
      }
    }
    stage('Upload Artifact') {
      steps {
        echo "${env.BUILD_NUMBER}"
        bat "curl -uadmin:APmUi9KMQQq8KMj7PERGoMaDHPszJ7nTW3mnz -T caseStudy.1.0.0.${env.BUILD_NUMBER}.zip \"http://localhost:8081/artifactory/generic-local/prod/caseStudy.1.0.0.${env.BUILD_NUMBER}.zip\""
      }
    }
  }
  environment {
    MSTest = 'C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\Common7\\IDE\\MSTest.exe'
  }
}
