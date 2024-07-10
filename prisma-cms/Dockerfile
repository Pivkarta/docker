FROM mhart/alpine-node:10 as build

ARG SITE=boilerplate

ARG ROLE

ARG endpoint
ENV endpoint $endpoint

ARG ENV_MODE
ENV ENV_MODE $ENV_MODE

ARG PUBLIC_URL
ENV PUBLIC_URL $PUBLIC_URL

RUN apk add bash 
RUN apk add mc 
RUN apk add curl 
RUN apk add python2
RUN apk add python3
RUN apk add make 
RUN apk add g++
RUN apk add git 

# RUN apk add --upgrade --no-cache vips-dev build-base \
#   --repository https://alpine.global.ssl.fastly.net/alpine/v3.10/community/

WORKDIR /tmp/build/
RUN npm i sharp@0.21.0

WORKDIR /www/${SITE}/

COPY ./${SITE} .
COPY ./bin/* /usr/bin/

# Установку зависимостей нельзя переносить в entrypoint,
# потому что тот скрипт срабатывает уже тогда, когда контейнер создан и заменен имеющийся (если уже был запущен)
# RUN yarn install --ignore-engines
COPY ./data/node_modules.tar.gz .
RUN tar -xzf node_modules.tar.gz
RUN rm node_modules.tar.gz
RUN rm ./node_modules/sharp -rf

RUN mv /tmp/build/node_modules/sharp ./node_modules

# RUN if [ "$ROLE" = "FRONT" ] ; then echo Build front ; /usr/bin/front.sh ; else echo ROLE is $ROLE ; /usr/bin/deploy-api-schema.sh ; fi
RUN if [ "$ROLE" = "FRONT" ] ; then echo Build front ; /usr/bin/front.sh ; else echo ROLE is $ROLE ; fi
