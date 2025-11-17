# DevOps TP - Sintia NGono (sintiangono / sintia4) 🚀

**Pipeline CI/CD complet : Docker + Docker Compose + Jenkins + Slack Notifications + GitHub + Docker Hub**

## Applications Déployées

### 1. Site Statique (Nginx)
- **Description** : Un site web simple et rapide servi par Nginx Alpine.
- **Contenu** : Page d'accueil avec "Mon Site Statique sur Docker!".
- **URL locale** : http://localhost:8080
- **Image Docker** : [sintia4/static-site:latest](https://hub.docker.com/r/sintia4/static-site)

![Capture Site Statique](https://raw.githubusercontent.com/sintiangono/devops-tp/main/capture-static.png)
*(Ajoute ta capture ici après l'étape 2)*

### 2. Application Flask (Python)
- **Description** : App web légère en Python avec route '/' custom.
- **Message affiché** : **Amen Thompson Dunk sur Docker + Flask ! 🏀**
- **URL locale** : http://localhost:8085
- **Image Docker** : [sintia4/flask-app:latest](https://hub.docker.com/r/sintia4/flask-app)

![Capture Flask App](https://raw.githubusercontent.com/sintiangono/devops-tp/main/capture-flask.png)
*(Ajoute ta capture ici après l'étape 2)*

### 3. WordPress + Odoo (Scalabilité)
- **WordPress** : Blog CMS avec base MySQL → http://localhost:8092
- **Odoo** : 3 instances ERP (avec PostgreSQL partagé) :
  - Odoo 1 : http://localhost:8075
  - Odoo 2 & 3 : Scalables en arrière-plan
- **Docker Compose** : Tout en un fichier pour un démarrage facile.

## Installation & Lancement (1 commande)
```bash
git clone https://github.com/sintiangono/devops-tp.git
cd devops-tp
docker compose up -d --build
