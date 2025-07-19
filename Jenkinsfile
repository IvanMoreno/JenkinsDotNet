pipeline {
    agent any
    
    parameters {
        string(name: 'BUILD_VERSION', defaultValue: '1.0.0', description: 'Version number')
        choice(name: 'BUILD_CONFIG', choices: ['Release', 'Debug'], description: 'Build configuration')
        choice(name: 'TARGET_FRAMEWORK', choices: ['net6.0', 'net8.0', 'netcoreapp3.1'], description: 'Target framework')
    }
    
    environment {
        DOTNET_ROOT = "C:\\Program Files\\dotnet"
        PATH = "${DOTNET_ROOT};${env.PATH}"
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
                bat """
                    dotnet build --configuration ${params.BUILD_CONFIG} --no-restore -p:Version=${params.BUILD_VERSION}
                """
            }
        }
        
        stage('Test') {
            steps {
                bat """
                    dotnet test --configuration ${params.BUILD_CONFIG} --no-build --logger trx --results-directory TestResults
                """
                publishTestResults testResultsPattern: 'TestResults/*.trx'
            }
        }
        
        stage('Publish') {
            steps {
                bat """
                    if not exist "Publish" mkdir "Publish"
                    dotnet publish --configuration ${params.BUILD_CONFIG} --no-build --output "Publish\\MyApp_${params.BUILD_VERSION}" --framework ${params.TARGET_FRAMEWORK}
                """
                archiveArtifacts artifacts: 'Publish/**/*', fingerprint: true
            }
        }
    }
}