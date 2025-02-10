# OSM - Voyages et Partages

## 🚀 Présentation du projet

Ce projet a pour but de créer une carte interactive représentant un carnet de voyage avec des points d’intérêts (musées, points de vue…), des photos et les tracés piétons/voiture entre les points. Il est basé sur une architecture **FullStack** avec un **backend Spring Boot** et un **frontend Vue.js**, et exploite des outils avancés pour la gestion des données géospatiales et cartographiques.

### 🛠️ Stack utilisée

- **Backend** : Spring Boot, Hibernate, Hibernate Spatial, PostGIS, MapStruct, Actuator
- **Frontend** : Vue.js, TypeScript, Bootstrap 5, Vue Router (navigation), Fetch API (requêtes HTTP), Tailwind CSS (style)
- **Base de données** : PostgreSQL avec extension PostGIS
- **Infrastructure** : Docker, Docker Compose, Nginx(serveur web pour le frontend)
- **Services externes** : JawgMaps, MapLibre GL, Unsplash API, Forward Geocoding, Reverse Geocoding
- **CI/CD & Gestion de version** : Git, GitHub, GitHub Actions
- **Optimisation Docker** : Multistage build pour des images plus légères et optimisées

---

## 🚀 Exécution du projet

Le projet est disponible sur **GitHub** et utilise **Docker Hub** pour récupérer les images des conteneurs.

---

## 📦 1. Prérequis

- **Docker** installé
- **Docker Compose** installé
- **Git** installé

---

## ⏬ 2. Cloner le projet et configurer les variables d'environnement

Clonez le projet depuis GitHub :

```bash
git clone https://github.com/MohamedAliSidibe/OSM_Project_Repot_DockerHub.git
```

Modifiez le fichier `.env` avec vos propres valeurs :

```ini
# Base de données PostgreSQL
POSTGRES_DB=name_database
POSTGRES_USER=name_user
POSTGRES_PASSWORD=passwordpost

# pgAdmin
PGADMIN_DEFAULT_EMAIL=email@gmail.com
PGADMIN_DEFAULT_PASSWORD=passwordpg
```

---

## ⏬ 3. Récupérer les images depuis Docker Hub

Exécutez la commande suivante pour télécharger les images directement depuis Docker Hub :

```bash
docker compose pull
```

Cela récupérera les images nécessaires au bon fonctionnement du projet.

---

## 🚀 4. Lancer l'application

Démarrez tous les services avec :

```bash
docker compose up -d
```

### 📍 Les services disponibles :

- **Frontend** (servi via **Nginx**) : [http://localhost:5173](http://localhost:5173)
- **Backend API (Spring Boot)** : [http://localhost:8080](http://localhost:8080)
- **pgAdmin** : [http://localhost:5050](http://localhost:5050) (utiliser les identifiants définis dans `.env`)

---

## 🔥 5. Bonnes pratiques REST API

### ✔️ Structuration des endpoints

- **GET** `/voyages` → Récupérer tous les voyages
- **POST** `/voyages` → Ajouter un nouveau voyage
- **PUT** `/voyages/{id}` → Modifier un voyage existant
- **DELETE** `/voyages/{id}` → Supprimer un voyage

### ✔️ Gestion des erreurs

Les API doivent renvoyer des **statuts HTTP clairs** :

- `200 OK` → Succès
- `201 Created` → Ressource créée
- `400 Bad Request` → Erreur côté client
- `401 Unauthorized` → Accès non autorisé
- `404 Not Found` → Ressource inexistante
- `500 Internal Server Error` → Problème serveur

### ✔️ Documentation

- Les endpoints sont documentés avec **Swagger/OpenAPI** pour faciliter l’intégration.

---

## 🐳 6. Optimisation Docker avec Multistage Build

Le projet utilise un **build multi-étapes (Multistage Build)** pour optimiser la taille et les performances des images Docker. Cela permet :

1. **De réduire la taille des images Docker** en ne gardant que les fichiers nécessaires à l’exécution.
2. **D’accélérer le build et l’exécution** en séparant les étapes de compilation et d’exécution.
3. **D’améliorer la sécurité** en réduisant la surface d’attaque et en excluant les dépendances inutiles.

Exemple de **Dockerfile multi-étapes** utilisé pour le backend :

```dockerfile
FROM maven:3.9.9-eclipse-temurin-17 AS builder
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jre-alpine
WORKDIR /OSM-Voyages
COPY --from=builder /app/target/Voyages-et-Partages*.jar /OSM-Voyages/app.jar
EXPOSE 8080
CMD ["java", "-jar", "/OSM-Voyages/app.jar"]
```

Exemple de **Dockerfile multi-étapes** utilisé pour le frontend :

```dockerfile
# build stage
FROM node:lts-alpine as build-stage
WORKDIR /app
COPY package*.json ./
RUN npm ci --legacy-peer-deps
COPY . .
RUN npm run build

# production stage
FROM nginx:stable-alpine as production-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## 🐳 7. Images Docker sur Docker Hub

Les images utilisées pour ce projet sont disponibles ici :

- 🌍 **Frontend (Nginx + Vue.js)** : [`momojr/osm-voyage-frontend`](https://hub.docker.com/r/momojr/osm-voyage-frontend)
- 🌍 **Backend (Spring Boot)** : [`momojr/osm-voyage-service`](https://hub.docker.com/r/momojr/osm-voyage-service)

---

## ❌ 8. Arrêter et nettoyer

Arrêter les services et supprime les conteneurs:

```bash
docker compose down
```

Supprimer les volumes associés :

```bash
docker compose down -v
```

---

🎉 **Le projet est maintenant prêt à être exécuté !**

