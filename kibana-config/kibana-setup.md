
# Kibana Installation and Configuration Guide with Nginx Reverse Proxy

This guide walks you through the process of installing Kibana and setting up an Nginx reverse proxy to access it securely.

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Installing Kibana](#installing-kibana)
3. [Configuring Nginx as a Reverse Proxy](#configuring-nginx-as-a-reverse-proxy)
4. [Securing Kibana with Basic Authentication](#securing-kibana-with-basic-authentication)
5. [Configuring Nginx Server Block](#configuring-nginx-server-block)
6. [Enabling the New Configuration](#enabling-the-new-configuration)
7. [Adjusting Firewall Rules](#adjusting-firewall-rules)
8. [Accessing Kibana](#accessing-kibana)

## Prerequisites

- Elasticsearch should be installed and running on your system.
- Ensure you have sudo or root access to your Ubuntu system.

## Installing Kibana

1. Install Kibana using APT:

```bash
sudo apt install kibana
```

2. Enable and start the Kibana service:

```bash
sudo systemctl enable kibana
sudo systemctl start kibana
```

## Configuring Nginx as a Reverse Proxy

Kibana is configured to listen only on localhost, so we'll set up Nginx as a reverse proxy to allow external access.

## Securing Kibana with Basic Authentication

1. Create an administrative Kibana user:

```bash
echo "kibana-admin:`openssl passwd -apr1`" | sudo tee -a /etc/nginx/htpasswd.users
```

This command will prompt you to enter and confirm a password for the `kibana-admin` user.

## Configuring Nginx Server Block

1. Create a new Nginx server block file:

```bash
sudo nano /etc/nginx/sites-available/my_domain
```

Replace `my_domain` with your server's Fully Qualified Domain Name (FQDN) or IP address.

2. Add the following configuration to the file:

```nginx
server {
    listen 80;
    server_name my_domain;
    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/htpasswd.users;

    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Replace `my_domain` with your server's FQDN or IP address.

## Enabling the New Configuration

1. Create a symbolic link to enable the new site:

```bash
sudo ln -s /etc/nginx/sites-available/my_domain /etc/nginx/sites-enabled/my_domain
```

Replace `my_domain` with the name you used for your configuration file.

2. Test the Nginx configuration for syntax errors:

```bash
sudo nginx -t
```

3. If the syntax is OK, reload Nginx to apply the changes:

```bash
sudo systemctl reload nginx
```

## Adjusting Firewall Rules

If you're using UFW (Uncomplicated Firewall), adjust the rules to allow Nginx:

1. Allow Nginx Full profile (HTTP and HTTPS):

```bash
sudo ufw allow 'Nginx Full'
```

2. If you previously allowed only HTTP, you can now remove that rule:

```bash
sudo ufw delete allow 'Nginx HTTP'
```

## Accessing Kibana

Kibana should now be accessible via your domain or IP address. You can check the Kibana server's status page by navigating to:

```
http://my_domain/status
```

Replace `my_domain` with your server's FQDN or IP address. You'll be prompted to enter the login credentials you set up earlier.

## Conclusion

You have successfully installed Kibana and set up Nginx as a reverse proxy with basic authentication. For more advanced configuration and usage, refer to the [official Kibana documentation](https://www.elastic.co/guide/en/kibana/current/index.html).

Remember to keep your system and Elastic Stack components updated for optimal performance and security.
