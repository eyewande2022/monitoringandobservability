# APACHE - FILEBEAT



#### Update

`sudo apt update -y`

#### Install Java JRE/JDK installation

`java --version`

`sudo apt install default-jre -y`

`javac -version`

`sudo apt install default-jdk -y`

#### Elastic Search Installation

`curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg`

`echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list`


### Update and Install Filebeat
`sudo apt update -y`

`sudo apt install filebeat -y`

`sudo vi /etc/filebeat/filebeat.yml`

`sudo mv /etc/filebeat/modules.d/apache.yml.disabled /etc/filebeat/modules.d/apache.yml`

`sudo filebeat setup --pipelines --modules apache`



configure elasticsearch out `output.elasticsearch`


#### Start ,Enable and Check the status of Apache2 if its currently running
`sudo apt install apache2 -y`

`sudo systemctl restart apache2`

`sudo systemctl enable apache2`

`sudo systemctl status apache2`



#### Move files from  file beats 

`ls -latr /etc/filebeat/modules.d/`

`sudo mv /etc/filebeat/modules.d/system.yml.disabled /etc/filebeat/modules.d/system.yml`

`sudo mv /etc/filebeat/modules.d/apache.yml.disabled /etc/filebeat/modules.d/apache.yml`

`sudo filebeat modules enable apache`


#### Restart and Enable Filebeat
`sudo systemctl restart filebeat`

`sudo systemctl enable filebeat`

# Simulate Apache logs

`sudo apt install apache2-utils -y`

## Success

ab -n 1000 -c 100 http://localhost/

## Slow response

ab -n 100 -c 100 -l http://localhost/

# Concurrent request

ab -n 1000 -c 5 http://localhost/

# Inject errors

ab -n 1000 -c 100 http://localhost/nonexistent-resource

# Vary request payload

ab -n 100 -c 10 -p data.txt -T application/json http://localhost/
