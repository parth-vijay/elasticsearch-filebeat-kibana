# Elasticsearch-Filebeat-Kibana 

## Run docker-compose to launch Elasticsearch and Kibana containers:
    sudo docker-compose up
    sudo docker-compose up -d

## Test Elasticsearch and Kibana is running or not:
    https://localhost:9200 (Elasticsearch)
    https://localhost:5601 (Kibana)

## Enable ingest-geoip and ingest-user-agent (Already enable in docker container):
    sudo docker exec elasticsearch /usr/share/elasticsearch/bin/elasticsearch-plugin install --batch ingest-geoip
    sudo docker exec elasticsearch /usr/share/elasticsearch/bin/elasticsearch-plugin install --batch ingest-user-agent
    sudo docker exec elasticsearch ls /usr/share/elasticsearch/plugins

## Install Filebeat on Ubuntu:
    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
    sudo apt-get install apt-transport-https
    echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
    sudo apt-get update && sudo apt-get install filebeat

## Modify /etc/filebeat/filebeat.yml to set the connection information:
    output.elasticsearch:
        hosts: [localhost:9200]

    setup.kibana:
        host: "localhost:5601"

## List the module available in Filebeat:
    sudo filebeat modules list

## Enable and configure the apache module:
    sudo filebeat modules enable apache

## Configure /etc/filebeat/modules.d/apache.yml to set the logs path:
    - module: apache
      access:
        enabled: true
        var.paths: ["/var/log/apache2/access.log"]

      error:
        enabled: true
        var.paths: ["/var/log/apache2/error.log"]

## Set up Kibana dashboard for Apache:
    sudo filebeat setup
    sudo filebeat setup --dashboards
    sudo systemctl start filebeat
    sudo systemctl status filebeat
    sudo systemctl restart filebeat

    