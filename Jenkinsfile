

pipeline {
    agent any 

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Authenticate with Salesforce') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'salesforce-creds', passwordVariable: 'SFDC_PASSWORD', usernameVariable: 'SFDC_USERNAME')]) {
                        sh '''
                        sf login --instanceurl https://login.salesforce.com --clientid 3MVG9fe4g9fhX0E5fvG54i4vMnP_CJSHE4eCpslE1Vc7dum262ifc7r6MWbCaF0OO6AEdbCacKt.74yTDY_Vm --clientsecret 18A7BD2E71F5923D902BBDF5E83B5DEAC76251FC2954C6D97C67C813833D5648 --username $SFDC_USERNAME --password $SFDC_PASSWORD
                        '''
                    }
                }
            }
        }

        stage('Validate Deployment') {
            steps {
                sh 'sf deploy metadata preview -d path_to_your_metadata_folder'
            }
        }

        stage('Manual Deployment') {
            steps {
                input(message: 'Approve deployment?', ok: 'Deploy')
                sh 'sf deploy metadata -d path_to_your_metadata_folder'
            }
        }
    }

    post {
        always {
            echo 'Pipeline Execution Finished!'
        }
    }
}
