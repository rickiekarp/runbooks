# Update
## Update SSL Certificate

Execute:
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/nginx-selfsigned.key \
    -out /etc/ssl/certs/nginx-selfsigned.crt \
    -addext "subjectAltName=DNS:rickiekarp.net,DNS:*.rickiekarp.net"
```

Copy to ca-certs:
```
sudo cp /etc/ssl/certs/nginx-selfsigned.crt /usr/share/ca-certificates/
```

And follow the instructions on the screen.

# Create
## Set up SSL Certificate

Step 1: Create the SSL Certificate

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/nginx-selfsigned.key \
    -out /etc/ssl/certs/nginx-selfsigned.crt \
    -addext "subjectAltName=DNS:rickiekarp.net,DNS:*.rickiekarp.net"
```

Fill out the prompts appropriately. The most important line is the one that requests the Common Name (e.g. server FQDN or YOUR name). You need to enter the domain name associated with your server or, more likely, your server’s public IP address.


Step 2:

While we are using OpenSSL, we should also create a strong Diffie-Hellman group, which is used in negotiating Perfect Forward Secrecy with clients.

We can do this by typing:

```
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

Step 3:

Add the newly created certificate to the NGINX vhost as needed via the included snippets found in `deployments/module-deployment/service-nginx/snippets`.

```
include snippets/self-signed.conf;
include snippets/ssl-params.conf;
```

## Set up certificate as trusted on a host system

Copy the public half of your untrusted CA certificate into the CA certificate directory (as root)

```
cp nginx-selfsigned.crt /usr/share/ca-certificates/
```

Rebuild the directory with your certificate included:
```
sudo dpkg-reconfigure ca-certificates
```