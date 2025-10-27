# TP2 Docker MLOps – Iris Classifier

## 🔹 Objectif du projet

Déployer un service IA pour classifier les iris via :

- **API FastAPI** pour les prédictions.
- **Frontend React** pour l’interface utilisateur.
- **Redis** pour le cache des prédictions.
- **Prometheus & Grafana** pour le monitoring des métriques.

Chaque composant est containerisé et orchestré avec **Docker Compose**.
📝 Description

Ce projet illustre le déploiement d’un modèle de Machine Learning (RandomForest sur le dataset Iris) via une API FastAPI et un Frontend React, chaque service étant containerisé avec Docker et orchestré avec Docker Compose.

Des fonctionnalités avancées ont été ajoutées :

Cache Redis pour stocker les résultats de prédiction et améliorer la performance.

Monitoring avec Prometheus pour collecter les métriques.

Dashboard Grafana pour visualiser les métriques en temps réel.

📂 Structure du projet
TP2DockerMLOps-main/
├─ api/                   # API FastAPI
│  ├─ app/                # Code source de l'API
│  └─ Dockerfile
├─ frontend/              # Frontend React
│  └─ Dockerfile
├─ monitoring/
│  └─ prometheus.yml      # Configuration Prometheus
├─ docker-compose.yml
├─ requirements.txt
└─ README.md

⚡ Fonctionnement

Le modèle (model.joblib + model_metadata.json) est placé dans api/app/model/.

L’API charge le modèle et expose deux endpoints principaux :

Endpoint	Méthode	Description
/health	GET	Vérifie l’état du modèle et de l’API
/predict	POST	Reçoit des données Iris et renvoie la prédiction, les probabilités et le cache

Le Frontend React envoie les mesures (sepal/petal) et affiche la prédiction.

Prometheus collecte les métriques exposées par l’API via /metrics.

Grafana affiche les métriques dans un tableau de bord.

🐳 Prérequis

Docker
 installé

Docker Compose
 installé

Python 3.11+ (pour build local de l’API si besoin)

🚀 Installation et exécution

Cloner le projet

git clone https://github.com/Medash69/TP2DockerMLOps.git
cd TP2DockerMLOps-main


Construire et lancer tous les conteneurs

docker compose up --build


Cela lancera :

API FastAPI : http://localhost:8000/docs

Frontend React : http://localhost:5174

Redis : port 6379

Prometheus : http://localhost:9090

Grafana : http://localhost:3000

Tester l’API

curl -X GET http://localhost:8000/health

curl -X POST http://localhost:8000/predict \
-H "Content-Type: application/json" \
-d '{
      "items": [
        {"sepal_length": 5.1, "sepal_width": 3.5, "petal_length": 1.4, "petal_width": 0.2}
      ]
    }'

🔧 Monitoring
Prometheus

URL : http://localhost:9090

Test des targets : Targets → iris-api → api:8000

Grafana

URL : http://localhost:3000

Login : admin / admin

Data Source : Prometheus → http://prometheus:9090

Dashboard : créer un panel pour visualiser http_request_duration_seconds_count ou http_requests_total

⚙️ Configuration
Variables d’environnement
Service	Variable	Description
API	API_PORT	Port de l’API FastAPI (8000)
API	CORS_ORIGINS	Origines autorisées pour le CORS
API	REDIS_HOST	Hôte Redis (redis)
API	REDIS_PORT	Port Redis (6379)
Frontend	VITE_API_BASE	URL de base de l’API
💾 Cache Redis

Les résultats des prédictions sont stockés dans Redis pendant 1 heure pour éviter de recalculer les mêmes requêtes.

Clé : JSON des données d’entrée triées.


📊 Notes

Ce projet montre l’orchestration complète d’une application ML avec API + Frontend + Cache + Monitoring.

Les dashboards Grafana et les métriques Prometheus permettent de suivre la santé et les performances de l’API en temps réel.

🔹 Légende

Frontend → Interface utilisateur qui envoie des requêtes de prédiction à l’API.

API FastAPI → Traite les requêtes, utilise le modèle ML et Redis pour le cache.

Redis → Cache les prédictions pour réduire le temps de réponse.

Prometheus → Collecte les métriques exposées par l’API (/metrics).

Grafana → Lit les métriques de Prometheus et les affiche dans un dashboard.

Model.joblib → Modèle ML pré-entraîné et ses métadonnées.
