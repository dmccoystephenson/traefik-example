# dansplugins-traefik-example

This is a minimal example showing how to use Traefik with Let's Encrypt to automatically generate HTTPS certificates for `dansplugins.com` using Docker.

## ✅ Features

- Automatic TLS using ACME (Let’s Encrypt)
- HTTP-01 challenge over port 80
- Secure HTTPS reverse proxy for a simple Nginx container
- Docker provider (no need for Traefik dashboard or static config files)

## 📦 File Structure

.
├── docker-compose.yml
├── letsencrypt/
│   └── acme.json  ← required, must exist with chmod 600

## 🚀 Getting Started

1. Clone the repo

   git clone https://github.com/dmccoystephenson/dansplugins-traefik-example.git
   cd dansplugins-traefik-example

2. Create certificate storage file

   mkdir -p letsencrypt
   touch letsencrypt/acme.json
   chmod 600 letsencrypt/acme.json

3. Update email address

   Edit `docker-compose.yml` and replace the line:

   --certificatesresolvers.letsencrypt.acme.email=your-email@example.com

4. Point your domain

   Make sure `dansplugins.com` points to your server's public IP address using an A or CNAME record.

5. Start the stack

   docker compose up -d

6. Verify

   Visit https://dansplugins.com in your browser. You should see the Nginx welcome page with a valid HTTPS certificate issued by Let's Encrypt.

## 🔁 Certificate Renewal

Traefik will automatically renew the certificate before it expires. It stores the certs and ACME account details in `letsencrypt/acme.json`.

## 🛠 Notes

- Port 80 must be open for HTTP-01 challenges to succeed
- Port 443 must be open to serve HTTPS traffic
- If using a firewall or cloud provider, make sure those ports are allowed
- You can add additional domains by adjusting labels on the `webapp` service

## 🧼 Tear Down

   docker compose down

This will stop the containers but keep the certificates unless you delete `letsencrypt/acme.json`.