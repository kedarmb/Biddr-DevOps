FROM node:alpine3.11 as node

MAINTAINER kedar <kbelavanaki@thinkperfect.io>

RUN mkdir -p /usr/src/app

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

RUN $(npm bin)/ng build --prod

FROM nginx:1.17.6-alpine

COPY --from=node /usr/src/app/dist /usr/share/nginx/html
