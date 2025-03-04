# 🚀 Getting started with Strapi

Strapi comes with a full featured [Command Line Interface](https://docs.strapi.io/dev-docs/cli) (CLI) which lets you scaffold and manage your project in seconds.

### `develop`

Start your Strapi application with autoReload enabled. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-develop)

```
npm run develop
# or
yarn develop
```

### `start`

Start your Strapi application with autoReload disabled. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-start)

```
npm run start
# or
yarn start
```

### `build`

Build your admin panel. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-build)

```
npm run build
# or
yarn build
```

## ⚙️ Deployment

Strapi gives you many possible deployment options for your project including [Strapi Cloud](https://cloud.strapi.io). Browse the [deployment section of the documentation](https://docs.strapi.io/dev-docs/deployment) to find the best solution for your use case.

```
yarn strapi deploy
```

## 📚 Learn more

- [Resource center](https://strapi.io/resource-center) - Strapi resource center.
- [Strapi documentation](https://docs.strapi.io) - Official Strapi documentation.
- [Strapi tutorials](https://strapi.io/tutorials) - List of tutorials made by the core team and the community.
- [Strapi blog](https://strapi.io/blog) - Official Strapi blog containing articles made by the Strapi team and the community.
- [Changelog](https://strapi.io/changelog) - Find out about the Strapi product updates, new features and general improvements.

Feel free to check out the [Strapi GitHub repository](https://github.com/strapi/strapi). Your feedback and contributions are welcome!

## ✨ Community

- [Discord](https://discord.strapi.io) - Come chat with the Strapi community including the core team.
- [Forum](https://forum.strapi.io/) - Place to discuss, ask questions and find answers, show your Strapi project and get feedback or just talk with other Community members.
- [Awesome Strapi](https://github.com/strapi/awesome-strapi) - A curated list of awesome things related to Strapi.

---

<sub>🤫 Psst! [Strapi is hiring](https://strapi.io/careers).</sub>

# Strapi Railway Application

This is a Strapi application configured for deployment on Railway.

## Deployment Status

Last updated: March 4, 2024 - 17:00

## Environment Variables

- All environment variables are properly configured
- Database connection is set up with SSL
- Strapi admin panel is configured with Railway domain

# Déploiement de Strapi sur Railway

Ce guide explique comment déployer une application Strapi sur Railway avec une base de données PostgreSQL.

## Prérequis

- Un compte [Railway](https://railway.app/)
- Un compte [GitHub](https://github.com/)
- Node.js installé localement

## Étapes de déploiement

### 1. Création du projet Strapi

```bash
# Créer un nouveau projet Strapi
npx create-strapi-app@latest my-project
cd my-project

# Initialiser Git
git init
git add .
git commit -m "Initial commit"
```

### 2. Configuration de Railway

1. Créer un nouveau projet sur Railway
2. Ajouter un service PostgreSQL
3. Ajouter un service depuis GitHub
   - Sélectionner votre repository
   - Choisir la branche main

### 3. Configuration des variables d'environnement

#### Dans le service Strapi :

```env
# Server
HOST=0.0.0.0
PORT=1337

# Secrets
APP_KEYS="votre_app_keys"
API_TOKEN_SALT="votre_salt"
ADMIN_JWT_SECRET="votre_jwt_secret"
TRANSFER_TOKEN_SALT="votre_transfer_salt"
JWT_SECRET="votre_jwt_secret"

# Database
DATABASE_CLIENT="postgres"
DATABASE_URL=postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}@${RAILWAY_PRIVATE_DOMAIN}:5432/${POSTGRES_DB}
DATABASE_SSL=true
DATABASE_SSL_REJECT_UNAUTHORIZED=false

# Strapi
NODE_ENV="production"
STRAPI_ADMIN_PUBLIC_URL=https://${RAILWAY_PUBLIC_DOMAIN}
STRAPI_URL=https://${RAILWAY_PUBLIC_DOMAIN}
```

### 4. Configuration des fichiers

#### config/database.js

```javascript
module.exports = ({ env }) => ({
  connection: {
    client: "postgres",
    connection: {
      connectionString: env("DATABASE_URL"),
      ssl: {
        rejectUnauthorized: false,
      },
    },
    debug: false,
  },
});
```

#### railway.toml

```toml
[build]
builder = "nixpacks"
buildCommand = "npm install"

[deploy]
startCommand = "npm run start"
healthcheckPath = "/admin"
healthcheckTimeout = 100
restartPolicyType = "on_failure"

[deploy.envs]
NODE_ENV = "production"
```

## Déploiement

1. Pousser les modifications sur GitHub :

```bash
git add .
git commit -m "Configuration pour Railway"
git push origin main
```

2. Railway déploiera automatiquement l'application

## Accès à l'application

- L'application sera disponible à l'URL fournie par Railway
- Le panneau d'administration sera accessible à `/admin`
- Créer un compte administrateur lors de la première connexion

## Notes importantes

- Ne jamais commiter les fichiers .env
- Garder les secrets en sécurité
- Toujours utiliser SSL pour la base de données
- Vérifier les logs en cas d'erreur de déploiement

## Support

En cas de problème :

1. Vérifier les logs de déploiement
2. S'assurer que toutes les variables d'environnement sont correctes
3. Vérifier la connexion à la base de données
4. Consulter la [documentation Strapi](https://docs.strapi.io)

---

Dernière mise à jour : 4 mars 2024
