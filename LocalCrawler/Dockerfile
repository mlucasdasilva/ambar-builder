FROM node:8.10 as git

WORKDIR /

WORKDIR /
RUN git clone https://github.com/RD17/ambar.git


FROM node:8.10

# Set a timezone
ENV TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#COPY . .
COPY --from=git /ambar/LocalCrawler/. .
RUN yarn install
RUN yarn run build

CMD node --max-old-space-size=8096 dist

HEALTHCHECK --interval=5s --timeout=30s --retries=50 \
  CMD curl -f localhost:8082/api/ || exit 1
