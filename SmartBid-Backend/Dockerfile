FROM node:12

EXPOSE 3000

ARG NODE_ENV
ENV NODE_ENV $NODE_ENV

RUN mkdir /app
WORKDIR /app
COPY package.json yarn.lock /app/
RUN yarn --pure-lockfile
COPY . /app

CMD ["yarn", "start"]
