# 🚀 TP DevOps Complet : Build & Déploiement - Sintia NGono (sintiangono / sintia4)

**Projet intégral : Docker + Docker Compose + Jenkins CI/CD + Notifications Slack + GitHub + Docker Hub**  
*Date : 17 Novembre 2025 | Auteur : Sintia Gono | Licence : MIT*

## Overview des Services & Déploiements
Voici le stack complet déployé en local (ou sur serveur). Tout se lance en 1 commande !

| Service              | Description                                                                 | Port Local      | Image Docker Hub / Commande                  | Lien Live / Test                  |
|----------------------|-----------------------------------------------------------------------------|-----------------|----------------------------------------------|-----------------------------------|
| **Site Statique (Nginx)** | Site web simple avec page d'accueil "Mon Site Statique sur Docker!"         | 8080            | [sintia4/static-site:latest](https://hub.docker.com/r/sintia4/static-site)<br>`docker run -d -p 8080:80 sintia4/static-site:latest` | [http://localhost:8080](http://localhost:8080) |
| **App Flask (Python)** | App web Flask avec message custom : **Amen Thompson Dunk sur Docker + Flask ! 🏀** | 8085            | [sintia4/flask-app:latest](https://hub.docker.com/r/sintia4/flask-app)<br>`docker run -d -p 8085:5000 sintia4/flask-app:latest` | [http://localhost:8085](http://localhost:8085) |
| **WordPress**        | CMS blog avec base MySQL intégrée                                           | 8092            | wordpress:latest (via Compose)               | [http://localhost:8092](http://localhost:8092) |
| **Odoo (3 instances)** | ERP scalable (1 exposé, 2 en background) avec PostgreSQL partagé            | 8075 (Odoo1)    | odoo:18 (via Compose)                        | [http://localhost:8075](http://localhost:8075) |
| **Jenkins**          | Pipeline CI/CD pour auto-deploy sur push GitHub                            | 8080            | jenkins/jenkins:lts<br>`docker run -d -p 8080:8080 jenkins/jenkins:lts` | [http://localhost:8080](http://localhost:8080) |

## Captures d'Écran (Démos Live)
*(Upload tes captures ici pour les voir en grand !)*

![Site Statique en Live](https://raw.githubusercontent.com/sintiangono/devops-tp/main/capture-static.png)  
*Capture : Page Nginx sur localhost:8080*

![Flask App en Action 🏀](https://raw.githubusercontent.com/sintiangono/devops-tp/main/capture-flask.png)  
*Capture : Message dunk sur localhost:8085*

## Installation & Test (Étapes du TP)
1. **Clone le repo** : `git clone https://github.com/sintiangono/devops-tp.git && cd devops-tp`
2. **Lancement global** : `docker compose up -d --build` (construit et déploie tout !)
3. **Test individuel** : Utilise les commandes Docker run ci-dessus.
4. **Push images** : `docker push sintia4/static-site:latest` (déjà fait pour toi !)

## Pipeline Jenkins CI/CD (Automatisé)
Déclenché par commit/push sur `main` ou `dev`. Intègre Slack pour notifications step-by-step.

**Jenkinsfile** (extrait du repo) :
```groovy
pipeline {
    agent any
    environment {
        SLACK_WEBHOOK = credentials('slack-webhook')
    }
    stages {
        stage('🚀 Start') {
            steps { slackSend message: "Début du build - Branch: ${env.BRANCH}" }
        }
        stage('Clone') {
            steps { git url: 'https://github.com/sintiangono/devops-tp.git', branch: 'main' }
        }
        stage('Build') {
            steps { sh 'docker compose build' }
            post { success { slackSend message: "✅ Images construites !" } }
        }
        stage('Deploy') {
            steps { sh 'docker compose up -d' }
            post { success { slackSend message: "🚀 Déploiement OK ! Sites live." } }
        }
    }
    post {
        failure { slackSend message: "❌ Échec du pipeline - Vérifier logs !" }
    }
}
