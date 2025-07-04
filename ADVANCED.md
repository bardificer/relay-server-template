## Advanced Usage

### Persisting to S3

Our goal for this section is to set up all of the secrets that are required to run the Relay Server.

First, we need to launch our app with the S3 config.
```
fly launch --copy-config --no-deloy --config=fly.s3.toml
```

Next, we need an S3 bucket and credentials to access that bucket. You can find your own way here, but I recommend using `s3-credentials`.

```
pip install s3-credentials
```

Now run `s3-credentials` and capture the output. We'll immediately send this to fly.io encrypted secret storage.
```
S3_CREDS=$(s3-credentials create-bucket "relay-${APP_NAME}" --region us-west-2)

ACCESS_KEY=$(echo "$S3_CREDS" | python3 -c "import sys, json; print(json.load(sys.stdin)['AccessKeyId'])")
SECRET_KEY=$(echo "$S3_CREDS" | python3 -c "import sys, json; print(json.load(sys.stdin)['SecretAccessKey'])")
REGION=$(echo "$S3_CREDS" | python3 -c "import sys, json; print(json.load(sys.stdin)['Region'])")
BUCKET=$(echo "$S3_CREDS" | python3 -c "import sys, json; print(json.load(sys.stdin)['Bucket'])")

flyctl secrets set \
    AWS_ACCESS_KEY_ID="$ACCESS_KEY" \
    AWS_SECRET_ACCESS_KEY="$SECRET_KEY" \
    AWS_REGION="$REGION" \
    STORAGE_BUCKET="$BUCKET"
    Y_SWEET_STORE="s3://$BUCKET/"
```

### Using a custom domain

You can use `flyctl certs` to add a custom domain to your app.
Update the Y_SWEET_URL_PREFIX in your service and in the connection string that you provide us.


### Using the flycast private network
#### Release your public IPs
```
fly ips list -c myfly.toml 
# VERSION	IP                    	TYPE              	REGION	CREATED AT       
# v6     	<your public ipv6>    	public (dedicated)	global	28m36s ago      	
# v4     	<your public ipv4>    	public (shared)   	      	Jan 1 0001 00:00

fly ips release <your public ipv6>
fly ips release <your public ipv4>
```

#### Deploy with a flycast ip
```
fly secrets set Y_SWEET_URL_PREFIX=http://$APP_NAME.flycast:8080
fly deploy --flycast
```

#### Create a wireguard connection
```
flyctl wireguard create \
    personal \  # your fly.io org
    sjc         # choose a region close to you

sudo mv fly.conf /etc/wireguard/
sudo chown root:root /etc/wireguard/fly.conf
```

#### Connect
```
sudo wg-quick up fly
```

#### Let us know!
We will update the connection string that users will receive and will connect to.


### Using tailscale

If you set a tailscale [auth-key](https://tailscale.com/kb/1085/auth-keys) the Relay Server will automatically join your tailnet with the hostname relay-server.

```
fly secrets set TAILSCALE_AUTHKEY=<auth-key>
fly secrets set Y_SWEET_URL_PREFIX=relay-server.<your tailnet>.ts.net
```

Update us whenever you change the Y_SWEET_URL_PREFIX so we can update it in our system.
