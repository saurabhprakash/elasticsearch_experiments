### Response formatting options:
 * ?pretty=true
 * ?format=yaml
 * ?human=false => This makes sense when the stats results are being consumed by a monitoring tool, rather than intended for human consumption. The default for the human flag is false. On true, statistics are returned in a format suitable for humans.
 * filter_path: All REST APIs accept a filter_path parameter that can be used to reduce the response returned by Elasticsearch. 
   * Comma separated list of filters expressed with the dot notation
     ```
      - curl: curl -XGET 'localhost:9200/_search?q=elasticsearch&filter_path=took,hits.hits._id,hits.hits._score&pretty'
      - kibana console: GET /_search?q=elasticsearch&filter_path=took,hits.hits._id,hits.hits._score
     ```
   * * wildcard character to match any field or part of a field’s name:
     ```
      - curl: curl -XGET 'localhost:9200/_cluster/state?filter_path=metadata.indices.*.stat*&pretty'
      - kibana console: GET /_cluster/state?filter_path=metadata.indices.*.stat*
     ```
   * ** wildcard can be used to include fields without knowing the exact path of the field:
     ```
      - curl: curl -XGET 'localhost:9200/_cluster/state?filter_path=routing_table.indices.**.state&pretty'
      - kibana console: GET /_cluster/state?filter_path=routing_table.indices.**.state
     ```
   * exclude one or more fields by prefixing the filter with the char -:
     ```
      - curl: curl -XGET 'localhost:9200/_count?filter_path=-_shards&pretty'
      - kibana console: curl -XGET 'localhost:9200/_count?filter_path=-_shards&pretty'
     ```
   * both inclusive and exclusive filters can be combined in the same expression
     ```
      - curl: curl -XGET 'localhost:9200/_cluster/state?filter_path=metadata.indices.*.state,-metadata.indices.logstash-*&pretty'
      - kibana console: GET /_cluster/state?filter_path=metadata.indices.*.state,-metadata.indices.logstash-*
     ```
  * flat_settings: flag affects rendering of the lists of settings. When flat_settings flag is true settings are returned in a flat format:
    ```
      - curl: curl -XGET 'localhost:9200/twitter/_settings?flat_settings=true&pretty'
      - kibana console: GET twitter/_settings?flat_settings=true
    ```
  * error_trace: By default when a request returns an error Elasticsearch doesn’t include the stack trace of the error. You can enable that behavior by setting the error_trace url parameter to true.
    ```
      - curl: curl -XPOST 'localhost:9200/twitter/_search?size=surprise_me&error_trace=true&pretty'
      - kibana console: POST /twitter/_search?size=surprise_me&error_trace=true

    ```
