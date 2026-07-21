pipeline {

    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    environment {
        ANYPOINT_CLIENT_ID = credentials('anypoint-client-id')
        ANYPOINT_CLIENT_SECRET = credentials('anypoint-client-secret')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '''
                mvn -s settings.xml clean verify \
                -Danypoint.client_id=$ANYPOINT_CLIENT_ID \
                -Danypoint.client_secret=$ANYPOINT_CLIENT_SECRET
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                mvn -s settings.xml deploy \
                -DskipTests \
                -Danypoint.client_id=$ANYPOINT_CLIENT_ID \
                -Danypoint.client_secret=$ANYPOINT_CLIENT_SECRET \
                -Danypoint.environment=Sandbox \
                -Danypoint.target=Cloudhub-US-East-2
                '''
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
    }
}