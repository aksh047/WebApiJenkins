pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/aksh047/WebApiJenkins.git'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build'  // Use 'sh' instead of 'bat' if on Linux
            }
        }

        stage('Test') {
            steps {
                bat 'dotnet test'  // Runs unit tests
            }
        }

        stage('Publish') {
            steps {
                bat 'dotnet publish -c Release -o output'  // Publish app to output directory
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'output/**', fingerprint: true
            }
        }
    }
}
