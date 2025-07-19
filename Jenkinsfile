pipeline {
    agent any
    
    environment {
        DOTNET_ROOT = "C:\\Program Files\\dotnet"
        REPO_URL = "https://github.com/IvanMoreno/JenkinsDotNet.git"
    }
    
    stages {
        stage('Checkout') {
            steps {
                cleanWs()
                bat "git clone ${REPO_URL} ."
            }
        }
        
        stage('Restore') {
            steps {
                bat "dotnet restore"
            }
        }
        
        stage('Build') {
            steps {
                bat "dotnet build --configuration Release --no-restore"
            }
        }
        
        stage('Test') {
            steps {
                bat "dotnet test --configuration Release --no-build --logger trx --results-directory TestResults"
            }
        }
        
        stage('Publish') {
            steps {
                bat """
                    if not exist "Publish" mkdir "Publish"
                    dotnet publish --configuration Release --no-build --output "Publish\\JenkinsTest" --framework net6.0
                """
                archiveArtifacts artifacts: 'Publish/**/*', fingerprint: true
            }
        }
    }
}