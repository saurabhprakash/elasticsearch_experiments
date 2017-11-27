Collection of commands and utilities for running/using elasticsearch

### Major commands
  * Running elastic search as deamon: "./bin/elasticsearch -d -p pid"
  * Check elasticsearch running: "curl -XGET 'localhost:9200/?pretty'"
  * Elasticsearch config location: "$ES_HOME/config/elasticsearch.yml"
  * Any settings that can be specified in the config file can also be specified on the command line, using the -E syntax as follows: "./bin/elasticsearch -d -Ecluster.name=my_cluster -Enode.name=node_1" 
