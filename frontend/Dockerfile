FROM node:8.15.1-alpine

#RUN npm install webpack -g
#
#WORKDIR /app
#COPY . /app
#
#RUN npm install


COPY package.json yarn.lock /app/
RUN yarn install --pure-lockfile

COPY --from=builder /app/node_modules /app/node_modules
COPY . /app/

ENV NODE_ENV production
RUN yarn build

EXPOSE 3000
CMD ["yarn", "start"]
