pipeline {
    agent any
    tools{
        maven 'MAVEN_HOME'
    }
    stages {
        stage('code checkout') {
            steps {
                bat 'code checkout'
            }
        }
        stage('code clean') {
            steps {
                bat "mvn clean"
            }
        }
        stage('Test') {
            steps {
                bat "mvn test"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
        stage('Sonar analysis') {
            steps {
                 withSonarQubeEnv("SonarQube"){
			bat "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"						
                  }
            }
        }
		stage("Quality Gate"){
			steps{
			script{
				timeout(time: 1, unit: 'HOURS'){
					def qg = waitForQualityGate()
					if(qg.status != 'OK'){
						waitForQualityGate abortPipeline: TRUE
					}
					else{
						waitForQualityGate abortPipeline: FALSE
					}
				}
				}				
			}
		}
		stage('Build') {
            steps {
                bat "mvn install"
				}
		}
		stage('Upload to Artifactory'){
			steps{
				rtMavenDeployer (
					id: 'deployer',
					serverId: 'jenkins-integration',
					releaseRepo: 'jenkins-integration',
					snapshotRepo: 'snapshot-integration'
				)
				rtMavenRun (
					pom: 'pom.xml',
					goals: 'clean install',
					deployerId: 'deployer'
				)
				rtPublishBuildInfo (
					serverId: 'Artifactory-server'
				)
			}
			
		}
    }
}
