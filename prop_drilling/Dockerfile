# Usar una imagen base de Node.js LTS
FROM node:lts-alpine

# Establecer el directorio de trabajo en /app
WORKDIR /app

# Copiar el archivo package.json y package-lock.json
COPY package*.json ./

# Instalar las dependencias del proyecto
RUN npm install

# Copiar todo el código fuente del proyecto
COPY . .

# Compilar el proyecto
RUN npm run build

# Exponer el puerto en el que correrá el servidor
EXPOSE 3000

# Comando para iniciar el servidor de desarrollo de Vite
CMD ["npm", "run", "dev", "--", "--host"]
