# Docker + R2 + Tailscale with Tailscale Serve (https)

```auth.env
# Relay
Y_SWEET_AUTH=${AUTH_TOKEN}
Y_SWEET_URL_PREFIX=https://relay-server.${TAILNET_NAME}.ts.net

# Tailscale
TAILSCALE_AUTHKEY=${TAILSCALE_AUTHKEY}
TAILSCALE_USERSPACE_NETWORKING=true
TAILSCALE_SERVE=true

# Cloudflare R2
AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
AWS_REGION=auto
STORAGE_BUCKET=${BUCKET}
AWS_ENDPOINT_URL_S3=https://${CLOUDFLARE_ACCOUNT_ID}.r2.cloudflarestorage.com
Y_SWEET_STORE=s3://${BUCKET}/
```

```bash
docker run -d \
  --name relay-server \
  --env-file auth.env \
  docker.system3.md/relay-server
```
