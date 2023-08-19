# Kibana- (Using NGINX as a reverse proxy)


We should know that Kibana is configured in such a way to listen on localhost only  and because of this ,we  would have a need to create a REVERSE PROXY to allow external accesss to it and NGINX fits the purpose.Here we would be  installing NGINX and ensure its running and then  create NGINX server block on sites available thereby including your server name  .

Because Kibana is configured to only listen on localhost, we must set up a reverse proxy to allow external access to it. We will use Nginx for this purpose, which should already be installed on your server.

`sudo apt update -y`

`sudo apt install nginx -y`


### The following command will create the administrative Kibana user and password, and store them in the htpasswd.users file.

Ensure you put a password after running the command which you would need when you are accessing the kibana interface.

`echo "kibanaadmin:`openssl passwd -apr1`" | sudo tee -a /etc/nginx/htpasswd.users
kibanaadmin:$apr1$wBmd5JGw$Kogt.FuOFOTzeXY591kKB0`

`sudo vim /etc/nginx/sites-available/kibana`

```
server {
    listen 80;

    server_name  "your domain or ip address";

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

### Enable the new configuration by creating a symbolic link to the sites-enabled directory.

sudo ln -s /etc/nginx/sites-available/kibana /etc/nginx/sites-enabled/kibana


### To check for syntax errors
sudo nginx -t

### Reload Nginx and firewall should allow full access

`sudo systemctl reload nginx`

`sudo ufw allow 'Nginx Full'`

`sudo ufw delete allow 'Nginx HTTP'`


### Launch your domain or Public ip address 

http://your domain or IP address /status
