pipeline {
    agent any
    environment {
        SLACK_WEBHOOK = credentials('slack-webhook')  // Credential Slack configurée dans Jenkins
        BRANCH = 'main'
        REPO_URL = 'https://github.com/sintiangono/devops-tp.git'
    }
    stages {
        stage('Notify Start') {
            steps {
                // Notification Slack début du pipeline
                slackSend(channel: '#devops-tp', message: "?? Début du build - Branch: ${env.BRANCH}", color: 'good', tokenCredentialId: 'slack-webhook')
            }
        }
        stage('Clone Repo') {
            steps {
                // Cloner le repo GitHub
                git branch: "${env.BRANCH}", url: "${env.REPO_URL}"
                slackSend(channel: '#devops-tp', message: "? Repo cloné", color: 'good', tokenCredentialId: 'slack-webhook')
            }
        }
        stage('Build Docker Images') {
            steps {
                // Construire les images Docker
                powershell 'docker compose build'
                slackSend(channel: '#devops-tp', message: "??? Images construites", color: 'good', tokenCredentialId: 'slack-webhook')
            }
        }
        stage('Deploy Docker Containers') {
            steps {
                // Lancer les containers en arrière-plan
                powershell 'docker compose up -d'
                slackSend(channel: '#devops-tp', message: "?? Déploiement terminé !\n- Site: http://localhost:8080\n- Flask: http://localhost:8085", color: 'good', tokenCredentialId: 'slack-webhook')
            }
        }
    }
    post {
        failure {
            // Notification Slack en cas d'échec
            slackSend(channel: '#devops-tp', message: "? ÉCHEC du pipeline", color: 'danger', tokenCredentialId: 'slack-webhook')
        }
    }
}
