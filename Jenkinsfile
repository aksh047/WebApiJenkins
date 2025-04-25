pipeline {
    agent any // Ensure the agent is a Windows node

    environment {
        AZURE_CREDENTIALS_ID = 'jenkins-pipeline-sp'
        RESOURCE_GROUP = 'MynewResourceGroup'
        APP_SERVICE_NAME = 'mywebappnew12'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/aksh047/WebApiJenkins.git'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet restore'
                bat 'dotnet build --configuration Release'
                bat 'dotnet publish -c Release -o ./publish'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    script {
                        def azureCredentials = readJSON(text: env.AZURE_CREDENTIALS_ID)
                        def clientId = azureCredentials.clientId
                        def clientSecret = azureCredentials.clientSecret
                        def tenantId = azureCredentials.tenantId

                        bat """
                            az login --service-principal -u ${clientId} -p ${clientSecret} --tenant ${tenantId}
                            powershell Compress-Archive -Path ./publish/* -DestinationPath ./publish.zip -Force
                            az webapp deploy --resource-group ${RESOURCE_GROUP} --name ${APP_SERVICE_NAME} --src-path ./publish.zip --type zip
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
