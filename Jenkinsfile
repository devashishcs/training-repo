pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK17'
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
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to CloudHub') {
            steps {
                sh '''
                cat > jenkins-settings.xml << EOF
<settings>
    <servers>
        <server>
            <id>anypoint-exchange-v3</id>
            <username>~~~Client~~~</username>
            <password>${ANYPOINT_CLIENT_ID}~?~${ANYPOINT_CLIENT_SECRET}</password>
        </server>
    </servers>
</settings>
EOF
                mvn deploy -DmuleDeploy -s jenkins-settings.xml -Danypoint.client.id=$ANYPOINT_CLIENT_ID -Danypoint.client.secret=$ANYPOINT_CLIENT_SECRET
                '''
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
        always {
            sh 'rm -f jenkins-settings.xml'
        }
    }
}
