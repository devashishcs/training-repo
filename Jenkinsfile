pipeline {
    agent any

    tools {
        maven 'Maven3'   // must match the name configured in Jenkins Global Tool Configuration
        jdk 'JDK17'      // must match the name configured in Jenkins Global Tool Configuration
    }

    environment {
        ANYPOINT_USERNAME = credentials('anypoint-username')
        ANYPOINT_PASSWORD = credentials('anypoint-password')
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
                sh 'mvn deploy -DmuleDeploy -Danypoint.username=$ANYPOINT_USERNAME -Danypoint.password=$ANYPOINT_PASSWORD'
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