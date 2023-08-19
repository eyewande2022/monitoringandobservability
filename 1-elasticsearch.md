# Installation of Elastic Search 

### Update 

`sudo apt update -y`


### Java JRE/JDK installation

`java --version`

`sudo apt install default-jre -y`

`javac -version`

`sudo apt install default-jdk -y`


### Elastic Search Installation
`curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg`

`echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list`

`sudo apt update -y`

`sudo apt install elasticsearch -y`




### Edit the elastic.yml file and update the network host,discovery seed host and cluster initial nodes

`sudo vi /etc/elasticsearch/elasticsearch.yml`

network.host: 0.0.0.0

discovery.seed_hosts

cluster.initial_master_nodes


### Start ,Enable and Check the status of Elastic search if its currently running 
`sudo systemctl start elasticsearch`

`sudo systemctl enable elasticsearch`

`sudo systemctl status elasticsearch`
