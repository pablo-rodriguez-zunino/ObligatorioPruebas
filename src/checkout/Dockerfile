# ==========================================
# Etapa 1: Compilación (Builder)
# ==========================================
FROM node:26-alpine AS builder

WORKDIR /app

# Copiar archivos de configuración y la carpeta completa de Yarn
COPY package.json yarn.lock .yarnrc.yml ./
COPY .yarn ./.yarn

# Copiar el código fuente completo antes de instalar
COPY . .

# Instalar dependencias usando directamente el binario local de Yarn
RUN node .yarn/releases/yarn-4.11.0.cjs install

# Compilar el proyecto NestJS (genera la carpeta /dist)
RUN node .yarn/releases/yarn-4.11.0.cjs build

# ==========================================
# Etapa 2: Imagen final de producción (Runner)
# ==========================================
FROM node:26-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

# Copiar archivos esenciales y la carpeta .yarn necesaria para ejecutar
COPY package.json yarn.lock .yarnrc.yml ./
COPY --from=builder /app/.yarn ./.yarn
COPY --from=builder /app/dist ./dist

# Instalar únicamente dependencias de producción de forma limpia
RUN node .yarn/releases/yarn-4.11.0.cjs workspaces focus --production

# Puerto que expone tu microservicio NestJS
EXPOSE 8080

# Comando para iniciar la aplicación compilada
CMD ["node", "dist/main.js"]

