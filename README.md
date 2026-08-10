# Stacks

Friendly stacks to get your docker homelab up and running

## Features

- Run on Debian/Ubuntu Server with Docker on VM, VPS, LXC/LXD, SBC or cloud
- Traefik reverse proxy with Lets Encrypt DNS-01 challenge with Cloudflare for valid certs on your internal network
- Curated Docker Compose files for popular open source applications to get you up and running quickly

## Assumptions and Pre-requisites

- A valid registered domain with Cloudflare as external DNS provider
- API token for Cloudflare DNS API as describeed in the [Cloudflare Documentation](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)
- DNS A or CNAME records pointing to your apps (Note with DNS-01 ACME challenge, you do not need to configure an external record for services on your local network. Just use a local DNS server or hosts file.)
- Debian/Ubuntu host with Docker and Docker compose installed as described at [Get Docker](https://get.docker.com/).
- This repo is geared towards a homelab on a private network which is not published to the public internet. No special steps have been attmpted to secure services if published to the internet and this is left as an exericse to the homelabber who has this requirement. A mesh VPN such as Netbird or Tailscale is recommended  for remote access in most use cases.

## Quick Start

In your home directory

```bash
sudo apt install apache2-utils -y
git clone https://github.com/thefriendlymanual/stacks.git
cd ./stacks/traefik-cf
cp ./.env.example ./.env
```

Create bcrypt hash for your Traefik dashboard password (replace 'username' below with actual username)

```bash
echo $(htpasswd -nB username) | sed -e 's/\$/\$\$/g'
```
Keep a copy of the output for the next step. (Looks like ```username:$$2y$$05$$QjdQq4ZkEntTGnZSLt9eieUNaxQNykF9jfaj57jaNfx2hxn2aFOvq```)

Edit your .env file and replace the placeholder values with your actual environment variable values

```bash
nano .env
```

Start Traefik

```bash
docker compose up -d
```

Finally add a DNS or host file entry for your Traefik Dashboard (eg. https://traefik.yourdomain.com) and confirm you can log in.

To spin up the other stacks, cd to the app's folder, rename the .env.example to .env, edit the .env with your details and the run ```docker compose up -d```

## Conventions

The following Conventions are used in the stacks

- Traefik Labels are default with the assumption that Traefik is used as described in the Quick Start.
- Docker images are specified with the "latest' tag. Best practice in production is to pin this to a specific tag/version.
- Docker named volumes are used for persisting important app data. By default on Debian based systems these are located at ```/var/lib/docker/volumes/``` If your app data is important to you, you will want to back these up.
- Bind mounts in the folder for each app may be used for config data or data that is recreated by the app. So if you back up the ```~/stacks/``` folder and ```/var/lib/docker/volumes``` you should be good.

## Licensing

This reporsitory is licensed under the MIT License. This license applies to the content of this repository only. Please consult the official documentation of each application or component for its licensing terms.
