---
title: "How to Deploy n8n with Docker: A Step-by-Step Guide forSelf-Hosting"
categories: [DevOps, Docker]
tags: [n8n, Docker, Automation, Self-Hosting]
date: 2025-05-26 20:00:00 -0500
math: false
mermaid: false
pin: false
media_subpath: /assets/img/posts/how-to-deploy-n8n-with-docker-a-step-by-step-guide-for-self-hosting
image: how-to-deploy-n8n-with-docker-a-step-by-step-guide-for-self-hosting.webp
---

Large language models have rekindled interest in automation platforms. One of them is [n8n](https://n8n.io/), which gives technical teams the flexibility of no-code. This tool has 400+ integrations and native AI capabilities, allowing us to interact with LLMs using APIs or even our self-hosted models with [Ollama](https://ollama.com/).

In this blog post, we are going to deploy the community version of n8n on our own server. To accomplish this, make sure you have already installed [Docker](https://www.docker.com/) and [Docker Compose](https://docs.docker.com/compose/). It’s worth noting that this guide is intended for Linux users; however, it also works on Windows through [Docker Desktop](https://www.docker.com/products/docker-desktop/).

## Docker Compose File

Let’s get the official image from [Docker Hub](https://hub.docker.com/). As the documentation specifies, there are some environment variables we should review:

* **`WEBHOOK_URL`**: The URL that n8n will use to generate webhook URLs. This is important for webhooks to function correctly, especially if you are running n8n behind a reverse proxy or on a different domain.
* **`N8N_HOST`**: The IP address or domain name of the server where n8n is running.
* **`VUE_APP_URL_BASE_API`**: The base URL for the n8n API. This is used by the frontend to make API calls. It should match the `WEBHOOK_URL` and `N8N_HOST`.
* **`N8N_PROTOCOL`**: The protocol used by n8n, either `http` or `https`.
* **`GENERIC_TIMEZONE`**: Specifies the timezone that n8n should use. It affects scheduling.
* **`TZ`**: The system’s timezone. This controls the output of certain scripts and commands.

Now that we know the environment variables, we can create a `docker-compose.yml` file. This file defines the n8n service and its configuration. Below is an example of how to set it up:

```yml
services:
  n8n:
    container_name: n8n
    image: n8nio/n8n:latest
    restart: always
    environment:
      GENERIC_TIMEZONE: America/Guayaquil
      TZ: America/Guayaquil
    ports:
      - 5678:5678
    volumes:
      - ./n8n:/home/node/.n8n
```

### Reverse Proxy

If you are using a reverse proxy, you will need to set the `WEBHOOK_URL`, `N8N_HOST`, `VUE_APP_URL_BASE_API`, and `N8N_PROTOCOL` environment variables accordingly. Here is an example of how to modify the `docker-compose.yml` file for a reverse proxy setup:

```yml
services:
  n8n:
    container_name: n8n
    image: n8nio/n8n:latest
    restart: always
    environment:
      WEBHOOK_URL: https://<your-domain>/
      N8N_HOST: <your-domain>
      VUE_APP_URL_BASE_API: https://<your-domain>/
      N8N_PROTOCOL: https
      GENERIC_TIMEZONE: America/Guayaquil
      TZ: America/Guayaquil
    ports:
      - 5678:5678
    volumes:
      - ./n8n:/home/node/.n8n
```

## Deploy

Now that we have our `docker-compose.yml` file ready, we can deploy n8n. Open a terminal and navigate to the directory where you saved the `docker-compose.yml` file. Then, run the following command:

```bash
docker-compose up -d
```

This command will start the n8n service in detached mode. You can check the status of the service by running:

```bash
docker-compose ps
```

> It’s possible that your server requires you to use `sudo` before running Docker commands.

If everything is running correctly, you should see the n8n service listed as "Up".

## Access n8n

You can access n8n by opening your web browser and navigating to `http://<your-server-ip>:5678` or `https://<your-domain>` if you are using a reverse proxy. You should see the n8n interface, where you can start creating workflows and automations.

## Conclusion

In this blog post, we covered how to deploy n8n using Docker Compose. We also discussed the necessary environment variables and how to configure n8n for use with a reverse proxy. With n8n, you can automate tasks and integrate various services without writing code, making it a powerful tool for technical teams.

If you have any questions or need further assistance, feel free to leave a comment below.

## Additional Resources

* [n8n Documentation](https://docs.n8n.io/)
* [Docker Documentation](https://docs.docker.com/)
* [Docker Compose Documentation](https://docs.docker.com/compose/)
* [Ollama Documentation](https://ollama.com/docs)
