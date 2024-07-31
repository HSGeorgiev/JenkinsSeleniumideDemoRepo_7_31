pipeline {
    agent any

    stages {
        stage ("Checkout code") {
            // checkout the repository
            steps {
                git branch: 'main', url: 'https://github.com/HSGeorgiev/JenkinsSeleniumideDemoRepo_7_31'
            }
        }

        stage ("Set uo .Net core") {
            // Set uo .Net core
            steps {
                bat '''
                echo Downloading .Net 6 Sdk
                curl -l -o dotnet-sdk-6.0.424-win-x64.exe  https://download.visualstudio.microsoft.com/download/pr/23c7bf0d-e22d-4372-bcb2-292eb36a5238/11af494be409759f46b679ab22e65a58/dotnet-sdk-6.0.424-win-x64.exe
                echo Installing dotnet-sdk-6.0.424-win-x64.exe ...
                dotnet-sdk-6.0.424-win-x64.exe quiet /norestart
                '''
            }
        }

        stage ("Restoring Nuget Packeges") {
            // Restoring Nuget Packeges
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            }
        }

        stage ("Build") {
            // Build
            steps {
                bat 'dotnet build SeleniumIde.sln --configuration Release'
            }
        }

        stage ("Run tests") {
            // Run tests
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=Testresults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/Testresults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher', 
                testResultFile: '**/Testresults/*.trx'
            ])
        }
    }


}