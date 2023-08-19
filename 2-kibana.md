# Kibana

### Ensure to restart Elasticsearch Before and after installing Kibana



### Update

`sudo apt update -y`

### Java JRE/JDK installation

`java --version`

`sudo apt install default-jre -y`

`javac -version`

`sudo apt install default-jdk -y`


### Elastic Search Installation

####  Import the Elasticsearch public GPG key into APT
`curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg`

####  Add the Elastic source list to the sources.list.d directory, where APT will search for new sources:
`echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list`


#### Install ,Enable and  Start Kibana 

`sudo apt install kibana -y`

`sudo systemctl enable kibana`

`sudo systemctl start kibana`
