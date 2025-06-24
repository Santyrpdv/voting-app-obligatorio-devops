FROM node:18-slim

# Instalar NGINX y herramientas necesarias
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl tini nginx && \
    rm -rf /var/lib/apt/lists/*

# Crear directorio de la app
WORKDIR /usr/local/app

# Instalar dependencias globales y locales
RUN npm install -g nodemon

COPY package*.json ./
RUN npm ci && \
    npm cache clean --force && \
    mv node_modules /node_modules

COPY . .

# Copiar configuración de NGINX
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf

# Exponer el puerto que NGINX va a escuchar
ENV PORT=80
EXPOSE 80

# Entrypoint y CMD para iniciar NGINX y la app
ENTRYPOINT ["/usr/bin/tini", "--"]
CMD service nginx start && node server.js
