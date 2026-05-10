# Guide de Contribution

Merci de contribuer au projet Todo App ! Ce guide explique comment travailler sur ce dépôt.

---

## Prérequis

- Git installé
- Docker et Docker Compose installés
- Accès au dépôt GitHub

---

## Flux de travail

Nous utilisons un flux basé sur des branches de fonctionnalités :

```
feature/xxx  →  dev  →  main
     PR           PR
  (review)     (review)
```

> ⚠️ Ne jamais pousser directement sur `main` ou `dev`.

---

## Étapes pour contribuer

### 1. Cloner le dépôt

```bash
git clone https://github.com/rabye19/getting-started-todo-app-sample
cd getting-started-todo-app-sample
```

### 2. Créer une branche depuis `dev`

```bash
git checkout dev && git pull origin dev
git checkout -b feature/nom-de-la-fonctionnalite
```

### 3. Faire ses modifications et committer

```bash
git add .
git commit -m "type(scope): description courte"
git push origin feature/nom-de-la-fonctionnalite
```

### 4. Ouvrir une Pull Request

- Aller sur GitHub → **Pull requests** → **New pull request**
- **base** : `dev` — **compare** : `feature/nom-de-la-fonctionnalite`
- ⚠️ Vérifier que `base repository` est `rabye19/...` et non `dockersamples/...`
- Ajouter un titre clair et une description
- Demander une review

### 5. Après approbation

Merger la PR `feature/xxx` → `dev`, puis ouvrir une nouvelle PR `dev` → `main`.

> ⚠️ S'assurer que le runner GitHub Actions est lancé sur la VM avant de merger vers `main`.

---

## Convention des commits

| Préfixe | Usage |
|---------|-------|
| `feat:` | Nouvelle fonctionnalité |
| `fix:` | Correction de bug |
| `ci:` | Pipeline CI/CD |
| `docs:` | Documentation |
| `chore:` | Maintenance |

**Exemples :**
```
feat(backend): add todo deletion endpoint
fix(traefik): add IP routing rule for local deployment
docs: update README with monitoring section
ci: add prometheus metrics to CD pipeline
```

---

## Pipeline CI/CD

Chaque push déclenche automatiquement la CI (lint, tests, build Docker).

Le déploiement (CD) se déclenche uniquement sur `main` via le runner self-hosted.

Pour lancer le runner manuellement sur la VM :
```bash
cd ~/actions-runner && ./run.sh
```

---

## Branches protégées

| Branche | Protection |
|---------|------------|
| `main` | PR obligatoire + 1 reviewer |
| `dev` | PR obligatoire |

---

## Contact

- **Rabye Trabelsi** — rabye19 (GitHub)
- **Asma Krimi** — asmakrimi263-rgb (GitHub)
