FROM node:latest as build-stage

# instalar un simple servidor http para servir nuestro contenido estático
#RUN npm install -g http-server

# hacer la carpeta 'app' el directorio de trabajo actual
WORKDIR /app

# copiar 'package.json' y 'package-lock.json' (si están disponibles)
COPY package*.json ./

# instalar dependencias del proyecto
RUN npm install

# copiar los archivos y carpetas del proyecto al directorio de trabajo actual (es decir, la carpeta 'app')
COPY . .
RUN npm i @vue/cli-service
ENV NODE_OPTIONS=--openssl-legacy-provider

# construir aplicación para producción minificada
RUN npm run build

#EXPOSE 8080
#CMD [ "http-server", "dist" ]


# etapa de producción
FROM nginx:1.13.12-alpine as production-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

# construir imagen y lanzarla
# docker build -t demotiendapp .
# docker run -it -p 8080:8080 --rm --name demotiendapp-1 demotiendapp