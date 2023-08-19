# Kibana

# Ensure to restart Elasticsearch Before and after installing Kibana

sudo apt update -y
java --version
sudo apt install default-jre -y
javac -version
sudo apt install default-jdk -y
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg

echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

sudo apt install kibana -y
sudo systemctl enable kibana
sudo systemctl start kibana
