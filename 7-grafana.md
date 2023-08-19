# Install Grafana

wget -q -O - https://packages.grafana.com/gpg.key | gpg --dearmor | sudo tee /usr/share/keyrings/grafana.gpg > /dev/null

# Next, add the Grafana repository to your APT sources:

echo "deb [signed-by=/usr/share/keyrings/grafana.gpg] https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

sudo apt update -y
sudo apt install grafana -y
sudo systemctl start grafana-server
sudo systemctl status grafana-server
sudo systemctl enable grafana-server

http://100.26.151.1:3000/
