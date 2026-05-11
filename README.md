# Todo App — Projet DevOps

**Rabye Trabelsi & Asma Krimi** | Enseignant : Hatem Hamdi | 2025-2026

Application de gestion de tâches déployée avec une pipeline CI/CD complète et un système de monitoring.

---

## Stack technique

| Composant | Technologie |
|-----------|-------------|
| Frontend | React + Vite |
| Backend | Node.js + Express |
| Base de données | MySQL 8.0 |
| Reverse proxy | Traefik v2.11 |
| Administration BDD | phpMyAdmin |
| Monitoring | Prometheus + Grafana |
| CI/CD | GitHub Actions |
| Conteneurisation | Docker + Docker Compose |

---

## Prérequis

- Docker et Docker Compose installés
- Git

---

## Installation et lancement

### 1. Cloner le dépôt

```bash
git clone https://github.com/rabye19/getting-started-todo-app-sample
cd getting-started-todo-app-sample
```

### 2. Créer le fichier `.env`

```bash
MYSQL_HOST=mysql
MYSQL_USER=root
MYSQL_PASSWORD=secret
MYSQL_ROOT_PASSWORD=secret
MYSQL_DATABASE=todos
MYSQL_DB=todos
```

### 3. Lancer l'application

```bash
docker compose up -d
```

---

## URLs d'accès

| Service | URL |
|---------|-----|
| Application | `http://192.168.1.135` |
| phpMyAdmin | `http://db.localhost` |
| Prometheus | `http://192.168.1.135/prometheus` |
| Grafana | `http://192.168.1.135/grafana` |

> Grafana — login : `admin` / mot de passe : `admin`

---

## Pipeline CI/CD

### CI (Intégration Continue)
Déclenchée sur chaque push vers `main`, `dev` ou `feature/**` :
- Lint backend et frontend (ESLint)
- Tests backend
- Build frontend (Vite)
- Build et push des images Docker sur Docker Hub

### CD (Déploiement Continu)
Déclenchée sur chaque push vers `main` :
- Déploiement automatique sur la VM via un runner self-hosted
- Mise à jour des containers Docker

---

## Flux de travail Git