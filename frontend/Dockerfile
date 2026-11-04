
# Stage 1 — build the app into static files
FROM node:22-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2 — minimal static server (no nginx, no npm, no vite)
FROM node:22-alpine
WORKDIR /app
RUN npm install -g serve@14 \
 && npm cache clean --force \
 && rm -rf /usr/local/lib/node_modules/npm \
           /usr/local/bin/npm /usr/local/bin/npx
COPY --from=build --chown=node:node /app/dist ./dist
USER node
EXPOSE 4173
CMD ["serve", "-s", "dist", "-l", "4173"]