# ETAPA 1: Construcción (Multi-stage solicitado en la pauta)
FROM alpine:latest AS build
WORKDIR /app
COPY index.html .

# ETAPA 2: Servidor de Producción con Mínimo Privilegio (Usuario no Root)
FROM nginx:alpine

# Proxy Inverso Corregido: Quita el prefijo /api/ al pasar la petición al backend
RUN echo 'server { \
    listen 80; \
    location / { \
        root /usr/share/nginx/html; \
        index index.html; \
    } \
    location /api/ { \
        proxy_pass http://innovatech-back:5000/; \
        proxy_set_header Host $host; \
        proxy_set_header X-Real-IP $remote_addr; \
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; \
    } \
}' > /etc/nginx/conf.d/default.conf

COPY --from=build /app/index.html /usr/share/nginx/html/index.html

# REQUERIMIENTO DE SEGURIDAD: Modificar permisos para usar usuario sin privilegios
RUN touch /var/run/nginx.pid && \
    chown -R nginx:nginx /var/run/nginx.pid /var/cache/nginx /var/log/nginx /etc/nginx

USER nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]