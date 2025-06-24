FROM python:3.11-slim AS base

# Instalar NGINX y dependencias
RUN apt-get update && \
    apt-get install -y nginx curl && \
    rm -rf /var/lib/apt/lists/*

# Crear directorios
WORKDIR /usr/src/app

# Copiar el código Python
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .

# Copiar configuración de NGINX
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf

# Flask se ejecutará en segundo plano
CMD service nginx start && gunicorn app:app -b 127.0.0.1:5000 --workers 4
