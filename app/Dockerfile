# Imagen base liviana de Python
FROM python:3.12-slim

WORKDIR /app

# Instalar dependencias
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copiar el código de la aplicación
COPY main.py .

# Crear directorio de datos y usuario sin privilegios
RUN mkdir -p /data \
    && useradd --create-home --uid 1000 appUsuario \
    && chown -R appUsuario /app /data

# Ejecutar como usuario no root
USER appUsuario

EXPOSE 8080
ENV DATA_DIR=/data PORT=8080

# Verificar que el servicio responde correctamente
HEALTHCHECK --interval=5s --timeout=3s --start-period=5s --retries=10 \
  CMD ["python", "-c", "import os, urllib.request; urllib.request.urlopen('http://localhost:%s/healthz' % os.environ.get('PORT', '8080'), timeout=2)"]

CMD ["python", "main.py"]