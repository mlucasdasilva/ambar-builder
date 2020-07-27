FROM kibana:5.6.3

# Set a timezone
ENV TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y curl

HEALTHCHECK --interval=5s --timeout=30s --retries=50 \
  CMD curl -f http://localhost:5601/ || exit 1
