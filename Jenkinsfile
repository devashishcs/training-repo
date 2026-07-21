pipeline {

    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                withCredentials([
                    string(credentialsId: 'anypoint-client-id', variable: 'ANYPOINT_CLIENT_ID'),
                    string(credentialsId: 'anypoint-client-secret', variable: 'ANYPOINT_CLIENT_SECRET')
                ]) {
                    sh '''
                    mvn -B -s settings.xml clean package \
                    -Danypoint.client_id=$ANYPOINT_CLIENT_ID \
                    -Danypoint.client_secret=$ANYPOINT_CLIENT_SECRET
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([
                    string(credentialsId: 'anypoint-client-id', variable: 'ANYPOINT_CLIENT_ID'),
                    string(credentialsId: 'anypoint-client-secret', variable: 'ANYPOINT_CLIENT_SECRET')
                ]) {
                    sh '''
                    mvn -B -s settings.xml clean deploy \
                    -DmuleDeploy \
                    -DskipTests \
                    -Danypoint.environment=Sandbox \
                    -Danypoint.target=Cloudhub-US-East-2 \
                    -Danypoint.client_id=$ANYPOINT_CLIENT_ID \
                    -Danypoint.client_secret=$ANYPOINT_CLIENT_SECRET
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful'
        }

        failure {
            echo 'Deployment Failed'
        }

        always {
            cleanWs()
        }
    }
}