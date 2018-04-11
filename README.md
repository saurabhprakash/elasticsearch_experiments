Collection of commands and utilities for running/using elasticsearch

Basic concepts: https://www.elastic.co/guide/en/elasticsearch/reference/current/_basic_concepts.html

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
 
The following settings must be addressed before going to production:

 * [Addtional system settings](https://www.elastic.co/guide/en/elasticsearch/reference/current/setting-system-settings.html): For increasing system resource to be used
 * [Set JVM heap size](https://www.elastic.co/guide/en/elasticsearch/reference/current/heap-size.html)
 * [Disable swapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-configuration-memory.html): Most operating systems try to use as much memory as possible for file system caches and eagerly swap out unused application memory. This can result in parts of the JVM heap or even its executable pages being swapped out to disk. Swapping is very bad for performance, for node stability, and should be avoided at all costs.
 * [Increase file descriptors](https://www.elastic.co/guide/en/elasticsearch/reference/current/file-descriptors.html): Elasticsearch uses a lot of file descriptors or file handles. Running out of file descriptors can be disastrous and will most probably lead to data loss.
 * [Ensure sufficient virtual memory](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html): Elasticsearch uses a mmapfs directory by default to store its indices.(more on mmapfs: https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-store.html, https://stackoverflow.com/questions/15187110/understanding-mmap)
 * [Ensure sufficient threads](https://www.elastic.co/guide/en/elasticsearch/reference/current/max-number-of-threads.html)

For more on above: https://www.elastic.co/guide/en/elasticsearch/reference/current/system-config.html

Elasticsearch bootstrap checks upon startup: https://www.elastic.co/guide/en/elasticsearch/reference/current/bootstrap-checks.html

### Important topics: 
  * Upgrade elasticsearch: https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-upgrade.html
  * X-Pack is an Elastic Stack extension that bundles security, alerting, monitoring, reporting, machine learning, and graph capabilities into one easy-to-install package: https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-xpack.html


### Important Note:Its not possible to create index of more than 1 type in elasticsearch version 6.x, reason explained in blog https://www.elastic.co/blog/index-type-parent-child-join-now-future-in-elasticsearch


Important Query readings:
 - Term Query: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html
 - Range Query: http://nocf-www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html
 - Match Query: http://nocf-www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html
 - Full text Queries: http://nocf-www.elastic.co/guide/en/elasticsearch/reference/current/full-text-queries.html
 - Finding multiple exact value: https://www.elastic.co/guide/en/elasticsearch/guide/current/_finding_multiple_exact_values.html
