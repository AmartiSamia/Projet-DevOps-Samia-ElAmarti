pipeline {
    agent any
    options { skipDefaultCheckout(true) }

    tools {
        maven 'M3'
    }

    environment {
        SLACK_WEBHOOK = credentials('SLACK_WEBHOOK')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            sh """
            curl -X POST -H 'Content-type: application/json' \
            --data '{"text":"✅ Pipeline Jenkins réussi : PipeLine-Samia-ElAmarti"}' \
            $SLACK_WEBHOOK
            """
        }
        failure {
            sh """
            curl -X POST -H 'Content-type: application/json' \
            --data '{"text":"❌ Pipeline Jenkins échoué : PipeLine-Samia-ElAmarti"}' \
            $SLACK_WEBHOOK
            """
        }
    }
}
