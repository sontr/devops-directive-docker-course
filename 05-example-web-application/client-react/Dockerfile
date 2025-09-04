FROM node:24.7.0-alpine AS build

WORKDIR /usr/src/app

COPY package*.json ./

RUN --mount=type=cache,target=/usr/src/app/.npm \
  npm set cache /usr/src/app/.npm && \
  npm install

COPY . .

RUN npm run build

FROM nginxinc/nginx-unprivileged:1.29.1-alpine

COPY --link nginx.conf /etc/nginx/conf.d/default.conf
COPY --link --from=build usr/src/app/dist /usr/share/nginx/html

EXPOSE 8080
