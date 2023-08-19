# Filebeat

### Update
`sudo apt update -y`

### Java JRE/JDK installation

`java --version`

`sudo apt install default-jre -y`

`javac -version`

`sudo apt install default-jdk -y`

### Elastic Search Installation

#### Import the Elasticsearch public GPG key into APT

`curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg`

#### Add the Elastic source list to the sources.list.d directory, where APT will search for new sources:

`echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list`

### Update

`sudo apt update -y`

#### Install Filebeat

`sudo apt install filebeat -y`

The Elastic Stack uses several lightweight data shippers called Beats to collect data from various sources and transport them to Logstash or Elasticsearch. Here are the Beats that are currently available from Elastic:

Filebeat: collects and ships log files.

Metricbeat: collects metrics from your systems and services.

Packetbeat: collects and analyzes network data.

Winlogbeat: collects Windows event logs.

Auditbeat: collects Linux audit framework data and monitors file integrity.

Heartbeat: monitors services for their availability with active probing.

In this tutorial we will use Filebeat to forward local logs to our Elastic Stack.

`sudo vi /etc/filebeat/filebeat.yml`

configure elasticsearch out `output.elasticsearch`

`sudo systemctl restart filebeat`

`sudo systemctl enable filebeat`

The functionality of Filebeat can be extended with Filebeat modules. In this tutorial we will use the system module, which collects and parses logs created by the system logging service of common Linux distributions.

Let’s enable it:

`ls -latr /etc/filebeat/modules.d/`

`sudo filebeat modules enable nginx`

`sudo mv /etc/filebeat/modules.d/system.yml.disabled /etc/filebeat/modules.d/system.yml`

`sudo mv /etc/filebeat/modules.d/nginx.yml.disabled /etc/filebeat/modules.d/nginx.yml`

`sudo filebeat modules enable nginx`

`sudo systemctl restart filebeat`

`sudo filebeat modules list`

By default, Filebeat is configured to use default paths for the syslog and authorization logs. In the case of this tutorial, you do not need to change anything in the configuration. You can see the parameters of the module in the /etc/filebeat/modules.d/system.yml configuration file.

Next, we need to set up the Filebeat ingest pipelines, which parse the log data before sending it through logstash to Elasticsearch. To load the ingest pipeline for the system module, enter the following command:

# Load dashboards in Kibana (Takes a bit of time to complete). Run it on the server where elasticsearch and kibana is running

`sudo filebeat setup -E output.logstash.enabled=false -E output.elasticsearch.hosts=['localhost:9200'] -E setup.kibana.host=localhost:5601`

# Simulate nginx logs

`sudo apt install apache2-utils -y`

## Success

ab -n 1000 -c 100 http://52.87.233.67/

## Slow response

ab -n 100 -c 100 -l http://52.87.233.67/

# Concurrent request

ab -n 1000 -c 5 http://52.87.233.67/

# Inject errors

ab -n 1000 -c 100 http://52.87.233.67/nonexistent-resource

# Vary request payload

ab -n 100 -c 10 -p data.txt -T application/json http://52.87.233.67/
