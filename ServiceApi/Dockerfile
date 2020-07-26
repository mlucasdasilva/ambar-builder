FROM node:8.10 as git

WORKDIR /

WORKDIR /
RUN git clone https://github.com/RD17/ambar.git



FROM node:8.10

# Set a timezone
ENV TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# COPY . .
COPY --from=git /ambar/ServiceApi/. .

RUN yarn install
RUN yarn run build

CMD node dist

HEALTHCHECK --interval=5s --timeout=30s --retries=50 \
  CMD curl -f localhost:8081/api || exit 1
