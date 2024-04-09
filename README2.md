# Adding Firewall and Reverse Proxy to Nginx Server
---
## Firewall Configuration with UFW

1. **Install UFW:**
    ```bash
    sudo pacman -S ufw
    ```

2. **Enable UFW:**
    ```bash
    sudo ufw enable
    ```

3. **Allow SSH Connections:**
    ```bash
    sudo ufw allow ssh
    ```

4. **Allow HTTP Connections:**
    ```bash
    sudo ufw allow http
    ```

## Backend Setup

1. **Transfer Backend Binary:**
    Transfer the `hello-server` binary to your server using SFTP or any other method.

2. **Place Backend Binary:**
    Place the `hello-server` binary in a logical location on your system.

3. **Create Service File:**
    Create a new service file to run the backend:
    ```bash
    sudo vim /etc/systemd/system/hello-server.service
    ```

    Add the following content to the service file:
    ```ini
    [Unit]
    Description=Hello Server
    After=network.target

    [Service]
    ExecStart=/path/to/hello-server
    Restart=always

    [Install]
    WantedBy=multi-user.target
    ```

    Replace `/path/to/hello-server` with the actual path where you placed the `hello-server` binary.

4. **Start Backend Service:**
    ```bash
    sudo systemctl start hello-server
    ```

    Ensure the service is running without errors:
    ```bash
    sudo systemctl status hello-server
    ```

## Configuring Reverse Proxy in Nginx

1. **Edit Nginx Server Block:**
    Open the Nginx server block configuration file:
    ```bash
    sudo vim /etc/nginx/sites-available/nginx-2420
    ```

2. **Add Reverse Proxy:**
    Add the reverse proxy configuration to the server block:
    ```nginx
    server {
        listen 80;
        server_name YOUR_SERVER_IP;

        root /web/html/nginx-2420;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }

        location /hey {
            proxy_pass http://127.0.0.1:8080;
        }
        location /echo {
            proxy_pass http://127.0.0.1:8080;
        }
    }
    ```

    Replace `YOUR_SERVER_IP` with your server's IP address.

3. **Test Connection to Backend:**
    Use `curl` or any similar tool to test connecting to your backend:
    ```bash
    curl http://YOUR_SERVER_IP/backend/hey
    ```

    You should receive a response from the backend.

    Example curl commands:
    ```bash
    curl http://YOUR_SERVER_IP/hey
    ```

    ```bash
    curl -X POST -H "Content-Type: application/json" -d '{"message": "Hello from your server"}' http://YOUR_SERVER_IP/backend/echo
    ```

## Firewall Verification

1. **Check Firewall Status:**
    ```bash
    sudo ufw status
    ```
