# Ollama Tunnel

A Docker-based containerized application that securely exposes the Ollama API to the web through a Cloudflare Tunnel.
This setup also uses Nginx for OAuth, providing protection against malicious actors.
The project leverages official Docker images from Cloudflare and Nginx, offering a secure and reliable
solution.

## Overview

When you expose your Ollama instance with this project, you can securely serve it as a backend service and leverage its
capabilities to create or use a variety of front ends.

For example, you can run an Android app such as [GPT Mobile](https://github.com/Taewan-P/gpt_mobile), or run a web app
such as [Open WebUI](https://github.com/open-webui/open-webui). Both can be configured to point to your Ollama instance
exposed with this project.

## Services

1. Nginx
    * Uses the official Nginx image
    * Works as proxy server for your Ollama
      API - [FAQ](https://github.com/ollama/ollama/blob/4100ed7bdd417ae6d25bf64467fb9df33f3f6525/docs/faq.md#how-can-i-use-ollama-with-a-proxy-server)
    * Adds OAuth to the Ollama API by using a [Bearer token](https://oauth.net/2/bearer-tokens/)

2. Cloudflare Tunnel
    * Uses the official Cloudflare image
    * Exposes Ollama to the web via a secure
      tunnel - [FAQ](https://github.com/ollama/ollama/blob/4100ed7bdd417ae6d25bf64467fb9df33f3f6525/docs/faq.md#how-can-i-use-ollama-with-cloudflare-tunnel)
    * Configured to run in the same network as nginx

## Requirements

- **Ollama** Running on your host machine at default port `11434`.
- **Podman or your preferred containerization tool**
- An valid [free](https://blog.cloudflare.com/tunnel-for-everyone/) **Cloudflare Tunnel** token.

If you don't have a Cloudflare Tunnel yet, follow the
official [documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-remote-tunnel/)
for installation instructions.

## Setup

1. **Clone the repository**
    ```bash
    git clone https://github.com/eleiton/ollama-tunnel.git
    cd ollama-tunnel
    ```

2. **Setup environment variables**

   This project comes with an example environment variables file called [example.env](example.env)
   Create a copy and name it .env:

   ```bash
   cp example.env .env
   ```

   Then edit the `.env` file providing your Cloudflare Tunnel Token and Ollama API key.  Your API key can be anything you like.  The standard fomat is `sk-xxxxx`.  You just need to make sure that when you call your API, you send this key as the Authorization token.

    ```bash
    OLLAMA_API_KEY=YOUR_CUSTOM_OLLAMA_API_KEY 
    TUNNEL_TOKEN=YOUR_CLOUDFLARE_TUNNEL_TOKEN
    ```
1. **Run the project**
    ```bash
    podman compose up
    ```

## Validate

Run this command to check if the service is up and running.

```bash
curl -i https://YOUR_DOMAIN/api/version \
-H "Authorization: Bearer YOUR_CUSTOM_OLLAMA_API_KEY"
```
