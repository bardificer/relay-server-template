# Docker + S3 + Tailscale with Tailscale Server (https)

```auth.env
# Relay
Y_SWEET_AUTH=${AUTH_TOKEN}
Y_SWEET_URL_PREFIX=https://relay-server.${TAILNET_NAME}.ts.net

# Tailscale
TAILSCALE_AUTHKEY=${TAILSCALE_AUTHKEY}
TAILSCALE_USERSPACE_NETWORKING=true
TAILSCALE_SERVE=true

# AWS S3
AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
AWS_REGION=${BUCKET_REGION}
STORAGE_BUCKET=${BUCKET}
Y_SWEET_STORE=s3://${BUCKET}/
```

```bash
docker run -d \
  --name relay-server \
  --env-file auth.env \
  docker.system3.md/relay-server
```
