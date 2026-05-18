# ETAPA 1: Construcción (Multi-stage solicitado en la pauta)
FROM alpine:latest AS build
WORKDIR /app
COPY index.html .

# ETAPA 2: Servidor de Producción con Mínimo Privilegio (Usuario no Root)
FROM nginx:alpine

# Copiamos directamente el index construido
COPY --from=build /app/index.html /usr/share/nginx/html/index.html

# REQUERIMIENTO DE SEGURIDAD: Modificar permisos para usar usuario sin privilegios
RUN touch /var/run/nginx.pid && \
    chown -R nginx:nginx /var/run/nginx.pid /var/cache/nginx /var/log/nginx /etc/nginx

# Creamos la configuración del Proxy Inverso de forma segura usando bloques nativos
RUN printf 'server {\n\
    listen 80;\n\
    location / {\n\
        root /usr/share/nginx/html;\n\
        index index.html;\n\
    }\n\
    location /api/ {\n\
        proxy_pass http://innovatech-back:5000/;\n\
        proxy_http_version 1.1;\n\
    }\n\
}' > /etc/nginx/conf.d/default.conf

USER nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]