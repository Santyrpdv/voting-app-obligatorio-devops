FROM python:3.11-slim

# Instalar dependencias
RUN apt-get update && \
    apt-get install -y --no-install-recommends nginx curl && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/app

# Instalar dependencias de Python
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copiar código
COPY . .

# Copiar configuración nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Configurar NGINX
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# Expone el puerto 80 (nginx escucha ahí)
EXPOSE 80

# Ejecuta nginx y gunicorn juntos
CMD gunicorn app:app -b 127.0.0.1:5000 --workers 4 & nginx
