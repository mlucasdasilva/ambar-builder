# Multistage builder Option 1
# Stage 1:
FROM node:8.10 as compiler
WORKDIR /
RUN git clone https://github.com/RD17/ambar.git && \
    cd ambar/FrontEnd 
WORKDIR /ambar/FrontEnd
RUN npm install
#RUN npm run compile
# https://github.com/RD17/ambar/issues/284#issuecomment-665272272
RUN npm run deploy:prod


## Multistage builder Option 2
## https://github.com/RD17/ambar/issues/284#issuecomment-653785669
## Stage 1:
#FROM ubuntu:20.04 as build
#RUN apt update && apt install -y curl sudo
#RUN curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
#RUN apt update && apt install --no-install-recommends --no-install-suggests -y nodejs python2.7 build-essential
##WORKDIR /dist
##COPY . .
#WORKDIR /
#RUN apt install -y git && \
#    git clone https://github.com/RD17/ambar.git && \
#    cd ambar/FrontEnd 
#WORKDIR /ambar/FrontEnd
#RUN apt install -y python2
##RUN npm install && npm run compile
#RUN npm install
## https://github.com/RD17/ambar/issues/284#issuecomment-665272272
#RUN npm run deploy:prod



FROM nginx:latest

RUN apt-get update && apt-get install --no-install-recommends --no-install-suggests -y curl

# Set a timezone
ENV TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

COPY default /etc/nginx/conf.d/default.conf
#COPY dist /usr/share/nginx/html
# Multistage builder Option 1
COPY --from=compiler /ambar/FrontEnd/dist /usr/share/nginx/html
## Multistage builder Option 2
#COPY --from=builder /ambar/FrontEnd/dist /usr/share/nginx/html

CMD echo $api > /usr/share/nginx/html/apiUrl.txt && nginx -g "daemon off;"

HEALTHCHECK --interval=5s --timeout=30s --retries=50 \
  CMD curl -f localhost:80 || exit 1

