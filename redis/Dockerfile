FROM redis:6-alpine

COPY redis.conf .

ENTRYPOINT echo "requirepass $REDIS_PASSWORD" >> ./redis.conf && redis-server ./redis.conf
