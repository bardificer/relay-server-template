# Docker + R2 + Custom VPN

```auth.env
# Relay
Y_SWEET_AUTH=${AUTH_TOKEN}

## Set this to a server URL that will be accessible to users on the private network
## The default port is 8080 unless you are running a reverse proxy.
Y_SWEET_URL_PREFIX=${RELAY_SERVER_URL}

# Cloudflare R2
AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
AWS_REGION=us-east-1
AWS_S3_USE_PATH_STYLE: "true"
STORAGE_BUCKET=${BUCKET}
AWS_ENDPOINT_URL_S3=http://${MINIO_SERVER_IP}:9000
Y_SWEET_STORE=s3://${BUCKET}/

```

```bash
docker run -d \
  --name relay-server \
  --env-file auth.env \
  -p 8080:8080 \
  docker.system3.md/relay-server
```
