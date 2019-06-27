pipeline {
  agent any
  environment {
    MSTest = 'C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\Common7\\IDE\\MSTest.exe'
  } 
  stages {
    stage('Build, Test and Code Quality') {
      steps {
        withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarQube') {
          bat "\"${tool 'MSBuildSQ'}\"  begin /k:\"caseStudy\" /d:sonar.host.url=\"http://localhost:9000\" /d:sonar.login=\"b0730da2e5486dde54f6acb79e6740adee3615a8\" /v:'caseStudy.1.0.0.${env.BUILD_NUMBER}' /d:sonar.cs.vstest.reportsPaths='MSTestResults.trx'"
          bat(script: 'nuget restore "Case Study.sln" -source "https://api.nuget.org/v3/index.json"', label: 'Restore Packages', returnStatus: true)
          bat "\"${tool 'MSBuild'}\" \"Case Study.sln\" /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}"
<<<<<<< HEAD
          bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\Common7\\IDE\\MSTest.exe" /testcontainer:\'CaseStudy.Tests\\bin\\Debug\\CaseStudy.Tests.dll\' /resultsfile:MSTestResults.trx'
=======
          bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\Common7\\IDE\\MSTest.exe" /testcontainer:\'Case Study.Tests\\bin\\Release\\CaseStudy.Tests.dll\' /resultsfile:MSTestResults.trx'
>>>>>>> f1c56612a4302e13cd86ae221250e93149f569a9
          bat "\"${tool 'MSBuildSQ'}\" end /d:sonar.login=\"b0730da2e5486dde54f6acb79e6740adee3615a8\""
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
