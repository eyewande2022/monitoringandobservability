sudo apt update -y
sudo apt install snapd -y
sudo snap install nginx-prometheus-exporter --beta

cd /tmp
wget https://github.com/nginxinc/nginx-prometheus-exporter/releases/download/v0.11.0/nginx-prometheus-exporter_0.11.0_linux_amd64.tar.gz
tar -xvf nginx-prometheus-exporter_0.11.0_linux_amd64.tar.gz
sudo mv nginx-prometheus-exporter /usr/local/bin/

sudo vim /etc/nginx/sites-available/nginx_status

```
server {
    listen 127.0.0.1:8080;
    server_name localhost;
    location /nginx_status {
        stub_status on;
        access_log off;
        allow 127.0.0.1;
        deny all;
    }
}

```

sudo ln -s /etc/nginx/sites-available/nginx_status /etc/nginx/sites-enabled/nginx_status

sudo nginx -t
sudo systemctl reload nginx

sudo vim /etc/systemd/system/nginx-prometheus-exporter.service

[Unit]
Description=NGINX Prometheus Exporter
After=network.target

[Service]
ExecStart=/usr/local/bin/nginx-prometheus-exporter --nginx.scrape-uri="http://127.0.0.1:8080/nginx_status"
Restart=always
User=nginx
Group=nginx

[Install]
WantedBy=multi-user.target

sudo systemctl restart nginx-prometheus-exporter.service
sudo systemctl enable nginx-prometheus-exporter.service
sudo systemctl status nginx-prometheus-exporter.service

# Configure Prometheus to scrape the exporter endpoint

- job_name: kibana-nginx
  static_configs:
  - targets: ['54.236.48.16:9113']
    labels:
    alias: kibana-nginx

http://54.236.48.16:9113/metrics

Grafana Dashboard ID 14900

# Load script

#!/bin/bash

while true; do
echo "Running ab -n 1000 -c 100 http://localhost/"
ab -n 10000 -c 10000 http://localhost/

    echo "Running ab -n 100 -c 100 -l http://localhost/"
    ab -n 1000 -c 10000 -l http://localhost/

    echo "Running ab -n 1000 -c 5 http://localhost/"
    ab -n 10000 -c 50 http://localhost/

    echo "Running ab -n 1000 -c 100 http://localhost/nonexistent-resource"
    ab -n 10000 -c 10000 http://localhost/nonexistent-resource

    sleep 5  # Adjust the sleep interval as needed

done
