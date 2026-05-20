# Usa una imagen oficial de Node de Alpine para que sea liviana y segura
FROM node:18-alpine

# Crear el directorio de la aplicación internamente
WORKDIR /usr/src/app

# Copiar los archivos de dependencias
COPY package*.json ./

# Instalar las dependencias de producción
RUN npm install --only=production

# Copiar el resto del código del backend
COPY . .

# REQUERIMIENTO DE SEGURIDAD DE LA PAUTA: Cambiar permisos al usuario 'node' sin privilegios
RUN chown -R node:node /usr/src/app
USER node

# Exponer el puerto interno nativo del back
EXPOSE 8080

# Comando para iniciar la aplicación
CMD ["node", "server.js"]