	pipeline {
	  agent any
	  stages {
		stage('Build') {
		  steps {
			withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarQube') {
			  bat "\"${tool 'MSBuildSQ'}\"  begin /k:\"caseStudy\" /d:sonar.host.url=\"http://localhost:9000\" /d:sonar.login=\"b0730da2e5486dde54f6acb79e6740adee3615a8\" /v:'caseStudy.1.0.0.${env.BUILD_NUMBER}' /d:sonar.cs.vstest.reportsPaths='TestResults\\*.trx'"
			  bat(script: 'nuget restore "Case Study.sln" -source "https://api.nuget.org/v3/index.json"', label: 'Restore Packages', returnStatus: true)
			  bat "\"${tool 'MSBuild'}\" \"Case Study.sln\" /p:Configuration=Release /t:Build /p:OutDir=\"$WORKSPACE\\Release\";Configuration=Release"
			  script{
				  stage('Test & Code Quality') {
					  bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\Common7\\IDE\\Extensions\\TestPlatform\\vstest.console.exe" /Logger:trx "Release\\CaseStudy.Tests.dll"'
					  bat "\"${tool 'MSBuildSQ'}\" end /d:sonar.login=\"b0730da2e5486dde54f6acb79e6740adee3615a8\""
				  }
			  }
			}
		  }
		}
		stage('Quality Gate'){
		  steps{
			  script{
				  timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout

					if (waitForQualityGate().status != 'OK') {
					  error "Pipeline aborted due to quality gate failure: ${waitForQualityGate().status}"
					}
				  }}
			}
		}
		stage('Upload Artifact') {
		  steps {
			zip(archive: true, zipFile: "caseStudy.1.0.0.${env.BUILD_NUMBER}.zip", dir: 'Release/_PublishedWebsites/Case Study/')
			archiveArtifacts "caseStudy.1.0.0.${env.BUILD_NUMBER}.zip"
			bat "curl -uadmin:APmUi9KMQQq8KMj7PERGoMaDHPszJ7nTW3mnz -T caseStudy.1.0.0.${env.BUILD_NUMBER}.zip \"http://localhost:8081/artifactory/generic-local/prod/caseStudy.1.0.0.${env.BUILD_NUMBER}.zip\""
		  }
		}
		stage('Deploy to Staging') {
			steps{
			  script{

				  def remote = [:]
				  remote.name = 'ansibleServer'
				  remote.host = '127.0.0.1'
				  remote.allowAnyHosts = true
				  
				  withCredentials([usernamePassword(credentialsId: '2db92d5a-3f3b-4b2a-b83f-c2f2df009b0f', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
				  
					  remote.user = "$USERNAME"
					  remote.password = "$PASSWORD"
					  sshCommand remote: remote, command: "cd ansible ; ansible-playbook -i inventory master.yml --extra-vars \"version=1.0.0.${env.BUILD_NUMBER}\""
				  }
	  
				}
			}
		}
		
		
		stage('Approval') {
			steps {
				input "Deploy Version 1.0.0.${env.BUILD_NUMBER} to Production environment?"
			}
		}
		
		stage('Deploy to Prod') {
			steps {
				echo "Deployed to Staging"
			}
		}
	  }
	  environment {
		MSTest = 'C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\Common7\\IDE\\MSTest.exe'
	  }
	}
