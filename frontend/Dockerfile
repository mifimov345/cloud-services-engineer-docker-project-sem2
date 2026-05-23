FROM node:20-alpine AS builder

WORKDIR /src

ARG VUE_APP_API_URL=http://localhost:8081
ENV VUE_APP_API_URL=${VUE_APP_API_URL}

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build


FROM nginxinc/nginx-unprivileged:1.30-alpine

COPY --from=builder /src/dist /usr/share/nginx/html
COPY --from=builder /src/dist /usr/share/nginx/html/momo-store

USER 101

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD wget -qO- http://127.0.0.1:8080/ || exit 1