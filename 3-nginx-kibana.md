Because Kibana is configured to only listen on localhost, we must set up a reverse proxy to allow external access to it. We will use Nginx for this purpose, which should already be installed on your server.

sudo apt update -y
sudo apt install nginx -y
echo "kibanaadmin:`openssl passwd -apr1`" | sudo tee -a /etc/nginx/htpasswd.users
kibanaadmin:$apr1$wBmd5JGw$Kogt.FuOFOTzeXY591kKB0

sudo vim /etc/nginx/sites-available/kibana

```
server {
    listen 80;

    server_name 54.236.48.16;

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

sudo ln -s /etc/nginx/sites-available/kibana /etc/nginx/sites-enabled/kibana

sudo nginx -t
sudo systemctl reload nginx

sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'

http://54.236.48.16/status
