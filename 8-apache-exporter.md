### Apache Exporter

### Ubuntu / Debian

sudo apt update -y && sudo apt install wget curl -y

curl -s https://api.github.com/repos/Lusitaniae/apache_exporter/releases/latest|grep browser_download_url|grep linux-amd64|cut -d '"' -f 4|wget -qi -

tar xvf apache_exporter-_.linux-amd64.tar.gz
sudo cp apache_exporter-_.linux-amd64/apache_exporter /usr/local/bin
sudo chmod +x /usr/local/bin/apache_exporter

apache_exporter --version

```
apache_exporter, version 1.0.1 (branch: HEAD, revision: 92eeaa4d50aea1ba26b0392534a7b33fd19c9002)
  build user:       root@b0b85db987d3
  build date:       20230724-22:34:04
  go version:       go1.19.11
  platform:         linux/amd64
  tags:             netgo
```

sudo groupadd --system prometheus
sudo useradd -s /sbin/nologin --system -g prometheus prometheus

sudo vim /etc/systemd/system/apache_exporter.service

```
[Unit]
Description=Prometheus
Documentation=https://github.com/Lusitaniae/apache_exporter
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/usr/local/bin/apache_exporter \
  --insecure \
  --scrape_uri=http://localhost:9117/server-status/?auto \
  --web.listen-address=0.0.0.0:9117 \
  --telemetry.endpoint=/metrics

SyslogIdentifier=apache_exporter
Restart=always

[Install]
WantedBy=multi-user.target
```

The service will listen on port 9117, and metrics exposed on /metrics URI. If Apache metrics are not on http://localhost/server-status/?auto you’ll need to change the URL.

sudo systemctl daemon-reload
sudo systemctl start apache_exporter.service
sudo systemctl enable apache_exporter.service
sudo systemctl status apache_exporter.service

# On Prometheus update the targets and add apache2

- job_name: apache2
  static_configs:
  - targets: ['100.27.3.115:9117']
    labels:
    alias: server2-apache

sudo systemctl reload prometheus

Dashboard ID - 3894

# Simulate Apache load

sudo apt install apache2-utils -y

## Success

ab -n 1000 -c 100 http://localhost/

## Slow response

ab -n 100 -c 100 -l http://localhost/

# Concurrent request

ab -n 1000 -c 5 http://localhost/

# Inject errors

ab -n 1000 -c 100 http://localhost/nonexistent-resource

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
