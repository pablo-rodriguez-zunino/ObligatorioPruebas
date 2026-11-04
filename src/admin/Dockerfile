FROM node:26-alpine

# Actualiza npm y las dependencias que vienen incluidas con esa versión
RUN npm install -g npm@12.2.1

WORKDIR /app

COPY . .
RUN npm install
RUN npm run build

ENV PORT=8080
EXPOSE 8080

CMD ["node", "dist/app.js"]