Collection of commands and utilities for running/using elasticsearch

### Major commands
  * Running elastic search as deamon: "./bin/elasticsearch -d -p pid"
  * Check elasticsearch running: "curl -XGET 'localhost:9200/?pretty'"
  * Elasticsearch config location: "$ES_HOME/config/elasticsearch.yml"
  * Any settings that can be specified in the config file can also be specified on the command line, using the -E syntax as follows: "./bin/elasticsearch -d -Ecluster.name=my_cluster -Enode.name=node_1" 


Changes in "config/elasticsearch.yml" for data and logs path: In production use, you will almost certainly want to change the locations of the data and log folder:

path:
  ```logs: /var/log/elasticsearch
  data: /var/data/elasticsearch
  ```
The RPM and Debian distributions already use custom paths for data and logs.

The path.data settings can be set to multiple paths, in which case all paths will be used to store data (although the files belonging to a single shard will all be stored on the same data path):

path:
  ```
  data:
    - /mnt/elasticsearch_1
    - /mnt/elasticsearch_2
    - /mnt/elasticsearch_3
  ```
A node can only join a cluster when it shares its cluster.name with all the other nodes in the cluster. 
 ```
 cluster.name: logging-prod
 ```
 
