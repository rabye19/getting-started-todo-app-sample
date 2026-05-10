# Todo App — Projet DevOps

> Industrialisation DevOps d'une application Todo conteneurisée  
> Binôme : **Rabye Trabelsi** & **Asma Krimi**  
> Enseignant : Hatem Hamdi | Année académique 2025–2026

---

## Sommaire

- [Présentation](#présentation)
- [Architecture](#architecture)
- [Prérequis](#prérequis)
- [Installation et démarrage](#installation-et-démarrage)
- [Variables d'environnement](#variables-denvironnement)
- [Services et ports](#services-et-ports)
- [Gestion des branches](#gestion-des-branches)
- [Pipeline CI/CD](#pipeline-cicd)
- [Monitoring](#monitoring)
- [Contribuer](#contribuer)

---

## Présentation

Ce projet est basé sur le dépôt open source [`dockersamples/getting-started-todo-app-sample`](https://github.com/dockersamples/getting-started-todo-app-sample).

Il s'agit d'une application de gestion de tâches (Todo App) sur laquelle nous appliquons une chaîne DevOps complète :

- Planification avec **GitHub Projects**
- Intégration continue avec **GitHub Actions**
- Conteneurisation avec **Docker** et **Docker Compose**
- Monitoring avec **Prometheus** et **Grafana**

---

## Architecture

L'application est composée de 5 services Docker orchestrés avec Docker Compose :

```
┌─────────────────────────────────────────────────────────┐
│                    Navigateur client                     │
└───────────────────────┬─────────────────────────────────┘
                        │ http://localhost:80
                        ▼
┌─────────────────────────────────────────────────────────┐
│              proxy (Traefik v2.11)                       │
│  Route /api/* → backend  |  /* → client                 │
└──────────────┬──────────────────────┬───────────────────┘
               │                      │
               ▼                      ▼
┌──────────────────────┐   ┌──────────────────────┐
│  backend (Node.js)   │   │  client (React+Vite)  │
│  Express API REST    │   │  Interface utilisateur │
│  port interne : 3000 │   │  port interne : 5173  │
└──────────┬───────────┘   └──────────────────────┘
           │
           ▼
┌──────────────────────┐
│  mysql (MySQL 8.0)   │
│  Base de données     │
│  port interne : 3306 │
│  volume persistant   │
└──────────────────────┘

┌──────────────────────┐
│  phpmyadmin          │
│  Interface web MySQL │
│  http://db.localhost │
└──────────────────────┘
```

### Stack technique

| Composant | Technologie | Version |
|-----------|------------|---------|
| Frontend | React + Vite | 5.0.12 |
| Backend | Node.js + Express + Nodemon | 3.0.3 |
| Base de données | MySQL | 8.0.45 |
| Proxy | Traefik | 2.11 |
| Admin DB | phpMyAdmin | latest |
| Conteneurisation | Docker + Docker Compose | 28.5.1 |

---

## Prérequis

Avant de commencer, s'assurer d'avoir installé :

- [Git](https://git-scm.com/) >= 2.x
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) >= 4.x
- Docker Compose >= 2.x (inclus dans Docker Desktop)

---

## Installation et démarrage

### 1. Cloner le dépôt

```bash
git clone https://github.com/rabye19/getting-started-todo-app-sample.git
cd getting-started-todo-app-sample
```

### 2. Configurer les variables d'environnement

```bash
cp .env.example .env
# Modifier .env si nécessaire (les valeurs par défaut fonctionnent en dev)
```

### 3. Lancer l'application

```bash
docker compose up --build
```

> Le premier lancement télécharge les images Docker (~300 MB) et peut prendre 5 à 10 minutes.

### 4. Accéder à l'application

| Service | URL |
|---------|-----|
| Application Todo | http://localhost |
| phpMyAdmin (DB) | http://db.localhost |

### 5. Arrêter l'application

```bash
# Arrêter les conteneurs
docker compose down

# Arrêter et supprimer les données MySQL
docker compose down -v
```

---

## Variables d'environnement

Copier `.env.example` en `.env` et remplir les valeurs :

```env
# MySQL
MYSQL_HOST=mysql
MYSQL_USER=root
MYSQL_PASSWORD=secret
MYSQL_ROOT_PASSWORD=secret
MYSQL_DATABASE=todos
MYSQL_DB=todos
```

> **Important :** Le fichier `.env` est dans le `.gitignore` et ne doit jamais être commité. Seul `.env.example` est versionné.

---

## Services et ports

| Service | Port exposé | Description |
|---------|-------------|-------------|
| proxy | 80 | Reverse proxy Traefik — point d'entrée unique |
| client | 5173 (interne) | Frontend React servi par Vite |
| backend | 3000 (interne) | API REST Node.js |
| mysql | 3306 (interne) | Base de données MySQL |
| phpmyadmin | 80 (interne) | Interface web d'administration MySQL |

### Healthcheck MySQL

Le service `mysql` dispose d'un healthcheck qui vérifie que le serveur est prêt à accepter des connexions avant de démarrer le `backend` et `phpmyadmin` :

```yaml
healthcheck:
  test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
  interval: 5s
  timeout: 5s
  retries: 10
  start_period: 30s
```

---

## Gestion des branches

```
main     ← branche stable, protégée (PR obligatoire)
dev      ← branche d'intégration
feature/ ← branches de fonctionnalités (ex: feature/ci-pipeline)
```

### Workflow de contribution

```bash
# 1. Créer une branche feature depuis dev
git checkout dev
git checkout -b feature/ma-fonctionnalite

# 2. Développer et committer
git add .
git commit -m "feat: description de la fonctionnalité"

# 3. Pousser et ouvrir une Pull Request vers dev
git push origin feature/ma-fonctionnalite
```

Convention des messages de commit :

| Préfixe | Usage |
|---------|-------|
| `feat:` | Nouvelle fonctionnalité |
| `fix:` | Correction de bug |
| `docs:` | Documentation |
| `ci:` | Pipeline CI/CD |
| `chore:` | Maintenance |

---

## Pipeline CI/CD

> En cours de mise en place — Phase 3 du projet

La pipeline GitHub Actions automatise :

- Build des images Docker (frontend + backend)
- Exécution des tests unitaires
- Analyse de qualité du code
- Publication des images sur Docker Hub
- Déploiement automatique sur merge vers `main`

---

## Monitoring

> En cours de mise en place — Phase 5 du projet

- **Prometheus** — collecte des métriques applicatives
- **Grafana** — visualisation et dashboards
- Alertes configurées sur : CPU > 80%, service down, latence élevée

---

## Contribuer

Voir [CONTRIBUTING.md](./CONTRIBUTING.md) pour les règles de contribution, conventions de branches et standards de code.

---

## Licence

Ce projet est basé sur [`dockersamples/getting-started-todo-app-sample`](https://github.com/dockersamples/getting-started-todo-app-sample).  
Voir [LICENSE](./LICENSE) pour les détails.
