pipeline {
    agent any

    tools {
        maven 'Maven3'   // must match the name configured in Jenkins Global Tool Configuration
        jdk 'JDK17'      // must match the name configured in Jenkins Global Tool Configuration
    }
	environment {
    ANYPOINT_CLIENT_ID = credentials('anypoint-client-id')
    ANYPOINT_CLIENT_SECRET = credentials('anypoint-client-secret')
}
    

    stages {
        stage('Checkout') {
            steps {
                git branch: 'evon-dc',
                    url: 'https://github.com/devashishcs/training-repo.git',
                    credentialsId: 'github-creds'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn deploy -DmuleDeploy -Danypoint.client.id=$ANYPOINT_CLIENT_ID -Danypoint.client.secret=$ANYPOINT_CLIENT_SECRET'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to CloudHub') {
            steps {
                sh 'mvn deploy -DmuleDeploy'
            }
        }
    }

    post {
        success {
            echo 'Deployment to CloudHub succeeded.'
        }
        failure {
            echo 'Pipeline failed — check logs above.'
        }
    }
}