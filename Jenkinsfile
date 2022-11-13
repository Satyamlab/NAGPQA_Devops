pipeline {
    agent any
    tools{
        maven 'MAVEN_HOME'
    }
    stages {
        stage('code checkout') {
            steps {
                code checkout
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
		stage('Build') {
            steps {
                bat "mvn install"
				}
		}
    }
}
