pipeline {
    agent any
    triggers {
        pollSCM('* * * * *') 
    }

    environment {
        AZURE_CREDENTIALS = credentials('azure-service-principal') 
        SUBSCRIPTION_ID = '5a53e0d2-b11c-4552-8fc7-775cb648da8e'
    }

    stages {
        stage('Parallel Deployments') {
            parallel {
                stage('Deploy India RG') {
                    steps {
                        dir('Template-RG-INDIA') {
                            sh '''
                            az login --service-principal -u $AZURE_CREDENTIALS_USR -p $AZURE_CREDENTIALS_PSW --tenant $AZURE_CREDENTIALS_TEN
                            az deployment sub create --location centralindia --template-file template.json --parameters @parameters.json --subscription $SUBSCRIPTION_ID
                            '''
                        }
                    }
                }
                stage('Deploy Canada RG') {
                    steps {
                        dir('Template-RG-CANADA') {
                            sh '''
                            az login --service-principal -u $AZURE_CREDENTIALS_USR -p $AZURE_CREDENTIALS_PSW --tenant $AZURE_CREDENTIALS_TEN
                            az deployment sub create --location canadacentral --template-file template.json --parameters @parameters.json --subscription $SUBSCRIPTION_ID
                            '''
                        }
                    }
                }
            }
        }

        stage('Deploy Load Balancer') {
            steps {
                dir('Template-LoadBalancer') {
                    sh '''
                    az login --service-principal -u $AZURE_CREDENTIALS_USR -p $AZURE_CREDENTIALS_PSW --tenant $AZURE_CREDENTIALS_TEN
                    az deployment sub create --location canadacentral --template-file template.json --parameters @parameters.json --subscription $SUBSCRIPTION_ID
                    '''
                }
            }
        }
    }
}
