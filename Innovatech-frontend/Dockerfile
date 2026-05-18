# ETAPA 1: Construcción (Multi-stage solicitado en la pauta)
FROM alpine:latest AS build
WORKDIR /app
COPY index.html .

# ETAPA 2: Servidor de Producción con Mínimo Privilegio (Usuario no Root)
FROM nginx:alpine

# Eliminar la configuración por defecto para evitar conflictos
RUN rm /etc/nginx/conf.d/default.conf

# Copiar nuestra configuración personalizada de Proxy Inverso
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copiar el HTML index generado en la etapa de build
COPY --from=build /app/index.html /usr/share/nginx/html/index.html

# REQUERIMIENTO DE SEGURIDAD: Modificar permisos para usar usuario sin privilegios
RUN touch /var/run/nginx.pid && \
    chown -R nginx:nginx /var/run/nginx.pid /var/cache/nginx /var/log/nginx /etc/nginx

USER nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]