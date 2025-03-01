# Nginx Notes

## What is Nginx?

Nginx (pronounced "Engine-X") is a high-performance web server, reverse proxy, load balancer, and caching system. It is widely used for serving web applications, handling API traffic, and improving website performance.

![](/images/nginx.png)

## Why Use Nginx?

✅ **High Performance**: Handles thousands of connections with minimal CPU usage.  
✅ **Reverse Proxy**: Forwards client requests to backend services.  
✅ **Load Balancer**: Distributes traffic among multiple servers for scalability.  
✅ **Static File Hosting**: Efficiently serves HTML, CSS, JS, and images.  
✅ **SSL Termination**: Manages HTTPS connections securely.  
✅ **Security Features**: Protects against DDoS attacks, implements access controls.  

## How Nginx Works

Nginx operates using an event-driven architecture that efficiently handles multiple requests asynchronously. This makes it superior to traditional web servers like Apache, which use a thread-based model.

### Basic Request Flow:

1. Client sends a request (e.g., `http://example.com`).
2. Nginx receives the request and decides how to process it.
3. Nginx either:
   - Serves a static file (e.g., `index.html`).
   - Passes the request to an upstream application (e.g., Frappe, Flask, Node.js).
   - Distributes the request across multiple backend servers (load balancing).
4. Nginx returns the response to the client.

## Installing Nginx

### On Linux (Ubuntu/Debian)
```sh
sudo apt update
sudo apt install nginx -y
```
Start and enable the service:
```sh
sudo systemctl start nginx
sudo systemctl enable nginx
```
Check if Nginx is running:
```sh
systemctl status nginx
```
Visit `http://localhost/` in your browser. You should see the Nginx welcome page.

### On CentOS/RHEL
```sh
sudo yum install epel-release -y
sudo yum install nginx -y
sudo systemctl start nginx
```

### Inside a Docker Container
```sh
docker run -d -p 80:80 nginx
```

## Nginx Configuration Basics

Nginx configuration files are located in:
- **Ubuntu/Debian**: `/etc/nginx/nginx.conf`
- **CentOS/RHEL**: `/etc/nginx/nginx.conf`
- **Docker**: `/etc/nginx/nginx.conf` (inside container)

Each website or application typically has its own configuration file inside `/etc/nginx/sites-available/` (on Ubuntu) or directly inside `/etc/nginx/conf.d/` (on CentOS/RHEL).

### Key Configuration Directives
```nginx
worker_processes auto;  # Defines the number of worker processes
error_log /var/log/nginx/error.log;  # Log errors

http {
    server {
        listen 80;  # Listen on port 80 (HTTP)
        server_name example.com;

        location / {
            root /var/www/html;
            index index.html;
        }
    }
}
```

## Using Nginx as a Reverse Proxy

A reverse proxy is a server that sits between client devices and backend servers, forwarding client requests to the appropriate backend application (e.g., Frappe, Flask, Node.js). It helps improve security, load balancing, and performance by acting as an intermediary that manages client-server communication.

![](/images/Forward-Proxy-vs-Reverse-Proxy-Servers.png)

### Example: Reverse Proxy for a Frappe App
```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:8000;  # Forward requests to Frappe
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
Restart Nginx after making changes:
```sh
sudo systemctl restart nginx
```

## Load Balancing with Nginx

If you have multiple backend servers, Nginx can distribute traffic among them.

### Example: Load Balancing Between Multiple Servers
```nginx
upstream backend_servers {
    server app1:8000;
    server app2:8000;
    server app3:8000;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend_servers;
    }
}
```
This will distribute incoming traffic across `app1`, `app2`, and `app3`.

## Securing Nginx with SSL (HTTPS)

You can add SSL/TLS encryption using Let's Encrypt.

### Step 1: Install Certbot
```sh
sudo apt install certbot python3-certbot-nginx -y
```
### Step 2: Generate SSL Certificate
```sh
sudo certbot --nginx -d yourdomain.com
```
Certbot will automatically update your Nginx configuration to use HTTPS.

### Manual SSL Configuration Example
```nginx
server {
    listen 443 ssl;
    server_name yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        proxy_pass http://127.0.0.1:8000;
    }
}
```

## Serving Static Files with Nginx

Nginx can serve static files directly, improving performance.

### Example: Hosting Static Files
```nginx
server {
    listen 80;
    server_name example.com;

    location /static/ {
        root /var/www/html;
    }
}
```
Place your static files in `/var/www/html/static/`.

## Useful Nginx Commands

| Command | Description |
|---------|-------------|
| `sudo systemctl start nginx` | Start Nginx |
| `sudo systemctl stop nginx` | Stop Nginx |
| `sudo systemctl restart nginx` | Restart Nginx |
| `sudo systemctl reload nginx` | Reload without downtime |
| `sudo nginx -t` | Test configuration for errors |
| `sudo tail -f /var/log/nginx/access.log` | View access logs |
| `sudo tail -f /var/log/nginx/error.log` | View error logs |