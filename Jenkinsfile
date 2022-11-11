pipeline {
    agent any
    tools{
        maven 'Maven'
    }
    stages {
        stage('code checkout') {
            steps {
                sh "echo hello"
            }
        }
        stage('code clean') {
            steps {
                sh "mvn clean"
            }
        }

        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
        stage('Sonar analysis') {
            steps {
                    withSonarQubeEnv("SonarQube")
                    {
                        sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
                    }
            }
         stage('Build') {
            steps {
                sh "mvn install"
            }
        }

        }
    }
}
