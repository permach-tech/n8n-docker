# n8n-docker

Self-hosting n8n instance in a docker container on a Raspberry Pi running Ubuntu 24.04.1 LTS

## Requirements

- Python 3.9 or higher
- Docker
- Device such as a RaspBerry Pi, mini PC, NUC, Laptop (or another affordable hosting provider like Render or Digital Ocean)

## Installation
   <details>
    <summary>Install Docker</summary>
    <br>
    
    # Add Docker's official GPG key:
   ```bash
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    ```
   
    # Add the repository to Apt sources:
   ```bash
    echo \
      "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    ```
   
    # Install the latest version
    ```bash
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```

   # Check Installed Version
    ```bash
    docker -v
    ```

    # Check Docker Compose

    ```bash
    docker compose
    ```

    # Check runtime

    ```bash
    sudo docker run hello-world
    ```

    # **Use Docker without sudo**

    ```bash
    sudo usermod -aG docker $USER
    ```
    </details>
    
    <details>
    <summary>n8n</summary>
    <br>

    # Create a directory for n8n
    The official docs for a self-hosted Docker instance can be found [here](https://docs.n8n.io/hosting/installation/docker/#starting-n8n/)

    # Create a directory for n8n
    ```bash
    mkdir n8n
    cd n8n
    ```

    n8n uses sqllite by default, but you can use Postgres or MySQL (recommended). For simplicity, we will use sqllite.

    A challenge I had was getting webhooks to work correctly, some of the docs recommend to start the n8n instance with the -tunnel environment variable, however, I was unable to get this working.

    I was running my n8n instance behind a cloudflare tunnel, so I had to run the docker command with the environment variable -e WEBHOOK_URL={your tunnel url}. If you are using a reverse proxy like ngnix or traefik, you may need to set this variable.
    List of environment variables can be found [here](https://docs.n8n.io/hosting/configuration/environment-variables/endpoints/)

    # Run the docker container with the environment variable
    ```bash
    docker run -d -it --rm --name n8n -e WEBHOOK_URL={your-url-here} -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n start
    ```
    </details>

    