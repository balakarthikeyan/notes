## Elasticsearch
Elasticsearch is primarily used as a search and analytics engine.
For logs management, Elasticsearch serves as the storage and search engine which indexes the log data received from Logstash, making it searchable and allowing for fast and efficient querying and analysis of the logs.

## Logstash
A log pipeline tool that accepts inputs from various sources, executes different transformations, and exports the data to various targets such as Elasticsearch.

## Kibana
Kibana is an open-source data visualization and exploration tool designed for use with Elasticsearch.
Kibana provides a user-friendly interface for exploring, analyzing, and visualizing data(logs) stored in Elasticsearch indices.

Confirm if ElasticSearch is running on http://localhost:9200/ and Kibana is running on http://localhost:5601/api/status
Verify your indexes are showing up on ElasticSearch as below : on url http://localhost:9200/_cat/indices
Create your index pattern from Menu → Stack Management → Index Pattern i.e http://localhost:5601/app/management/kibana/indexPatterns
Logstash: http://localhost:9600