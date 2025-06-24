FROM node:18-slim

# Instalar curl, nginx y tini
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl nginx tini && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/app

# Copiar y preparar dependencias
COPY package*.json ./
RUN npm ci && npm cache clean --force

COPY . .

# Copiar configuración nginx
COPY nginx.conf /etc/nginx/nginx.conf

# Definir puertos
ENV PORT=3000
EXPOSE 80

# Iniciar nginx y node
CMD service nginx start && node server.js
