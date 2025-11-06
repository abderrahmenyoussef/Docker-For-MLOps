# Étape 1 : Build de l'application React
FROM node:20-alpine AS builder

# Variables d'environnement pour optimiser le build
ENV NODE_ENV=development

# Répertoire de travail
WORKDIR /app

# Copier uniquement les fichiers nécessaires à l’installation
COPY package*.json ./

# Installer les dépendances sans cache et sans fichiers inutiles
RUN npm install

# Copier le reste du code source
COPY . .

# Compiler l'application React (production build)
RUN npm run build

# Étape 2 : Déploiement via Nginx
FROM nginx:alpine

# Nettoyer et sécuriser l'image finale
RUN rm -rf /var/cache/apk/* && \
    adduser -D -H -u 1001 frontenduser

# Copier la configuration nginx personnalisée
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copier les fichiers compilés du builder vers nginx
COPY --from=builder /app/dist /usr/share/nginx/html

# Exposer le port 80
EXPOSE 80

# Lancer nginx en mode foreground
CMD ["nginx", "-g", "daemon off;"]
