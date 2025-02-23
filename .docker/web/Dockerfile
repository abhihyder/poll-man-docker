# pull official base image
FROM node:20

ARG SERVER_ENVIRONMENT

ARG APP_DIR=/var/www/web
WORKDIR $APP_DIR


RUN npm install --global pm2
# RUN npm install --global yarn

# RUN chown www-data:www-data $APP_DIR

COPY ./web $APP_DIR

# RUN npm install
# RUN yarn cache clean

RUN yarn install -f

RUN yarn build

#EXPOSE 3000


# CMD HOST=0.0.0.0 pm2-runtime start yarn --interpreter bash --name web -- preview
CMD HOST=0.0.0.0 pm2-runtime start yarn --interpreter bash --name web -- start
