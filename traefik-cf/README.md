# Traefik with DNS-01 ACME Challenge via Cloudflare

[Traefik](https://doc.traefik.io/traefik/) (pronounced traffic) is a modern, open-source reverse proxy and load balancer designed for microservices and cloud-native environments. Unlike traditional web servers, it automatically discovers your backend services and updates its routing rules in real time as containers or services spin up and shut down.

## Key Features
- Dynamic Configuration: Automatically detects changes from platforms like Docker, Kubernetes, and Consul without needing manual reloads.
- Automatic HTTPS: Natively integrates with Let's Encrypt to generate and renew SSL/TLS certificates on the fly.Built-in 
- Middleware: Offers ready-to-use tools for rate limiting, basic authentication, request stripping, and retries.
- Web Dashboard: Provides a visual interface to monitor active routers, services, and entry points.

## Core Concepts
- Providers: The infrastructure sources (Docker, Kubernetes) that Traefik listens to for service updates.
- Entrypoints: The network doors (ports) on Traefik that accept incoming traffic.
- Routers: The logic mapping incoming requests to specific backend services based on rules.
- Services: The actual backend applications that handle the forwarded requests.

## Traefik in the Homelab

In the Homelab:

- Traefik can listen on port 443 and route or loadbalance to multiple services based on the SNI/fully qualified domain name. No need to expose ports for each service.
- Traefik can automatically obtain and renew valid Lets Encrypt certificates for each service
- Dynamically detects configuration for Docker services from Docker labels in the Compose file for that service.

## ACME DNS-01 Challenge

Traefik comes with the built in ability to automatically obtain and renew Lets Encrypt certificates for your services using the ACME protocol to validate domains.

Many people are familiar with using the TLS-01 ACME challenge to obtain a certificate but this method requires the service to be available over the internet. So it is not possible to use this challenge to obtain a certificate for a service on a private network.

When using the DNS-01 ACME challenge, Traefik uses your domain DNS provider's API to create a TXT record that Lets Encrypt can use to validate the domain. Because Traefik reaches out to the DNS provider's API, this challenge will work regardless of whether the service is available via the internet and also does not require a public DNS record for the service itself. This makes it perfect for services running on a private network

Our example configuration, we are using Cloudflare as our external DNS provider, which is a good choice or recommendation if you don't already have a preferred DNS proivder. However, Traefik can be configured to use any [DNS provider supported by the LEGO ACME client](https://go-acme.github.io/lego/dns/index.html) by modifying the environment variables in the Treafik .env and Docker Compose files.

## Learning Traefik

Traefik has a reputation for having a steep initial learning curve versus competing reverse proxies such as Nginx Proxy Manager or Caddy. The terminology and range of configuration options are quite challenging to get to grips with even for those with exetnsive technical experience.

However, our example config will work pretty much copy/paste to get Traefik up and running on any docker host. Traefik's out of the box capabilities surpas many of its competitors for Lets Encrypt and Docker integration. Traefik has also received decent coverage in recent years from Tou Tubers such as Jim's Garage, Christian Lempa and Techno Tim so there is much more reference material nowadays to help you get started.