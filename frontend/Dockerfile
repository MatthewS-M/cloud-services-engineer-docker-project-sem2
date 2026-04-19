# syntax=docker/dockerfile:1.7

FROM node:16-alpine AS builder

WORKDIR /app

COPY package.json package-lock.json ./
RUN --mount=type=cache,target=/root/.npm \
    npm ci

COPY . .

ARG VUE_APP_API_URL=http://localhost:8081
ARG VUE_APP_PUBLIC_PATH=/
ENV VUE_APP_API_URL=${VUE_APP_API_URL}
ENV VUE_APP_PUBLIC_PATH=${VUE_APP_PUBLIC_PATH}

RUN npm run build

FROM nginxinc/nginx-unprivileged:1.27-alpine

COPY nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]
