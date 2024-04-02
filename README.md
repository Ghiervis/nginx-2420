# Setting up Nginx Server on Arch Linux-DigitalOcean
-----
## Install Necessary Softwares

1. **Update Package Repositories:**
    ```bash
    sudo pacman -Syu
    ```

2. **Install Vim and Nginx:**
    ```bash
    sudo pacman -S vim nginx
    ```

## Nginx Configuration

1. **Create Project Directory:**
    ```bash
    sudo mkdir -p /web/html/nginx-2420
    ```

2. **Create Server Block Configuration File:**
    ```bash
    sudo vim /etc/nginx/sites-available/nginx-2420
    ```

    Add the following configuration:
    ```nginx
    server {
        listen 80;
        server_name YOUR_SERVER_IP;

        root /web/html/nginx-2420;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
    ```
    Replace `YOUR_SERVER_IP` with your server's IP address.

3. **Enable Server Block:**
    ```bash
    sudo ln -s /etc/nginx/sites-available/nginx-2420 /etc/nginx/sites-enabled/
    ```

4. **Test Nginx Configuration:**
    ```bash
    sudo nginx -t
    ```

5. **Restart Nginx:**
    ```bash
    sudo systemctl restart nginx
    ```

## Serving the Demo Document

1. **Create Demo Document:**
    ```bash
    sudo vim /web/html/nginx-2420/index.html
    ```

    Paste the provided HTML code into the file and save.

## Systemd Components

1. **Enable Nginx to Start on Boot:**
    ```bash
    sudo systemctl enable nginx
    ```
2. **Restart Nginx whenever making any changes like .conf file**
    ```bash
    sudo systemctl restart
    ```


