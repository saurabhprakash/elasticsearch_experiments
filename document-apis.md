##### Index apis (Reference: https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html)
  * Add or update a typed JSON document in a specific index, making it searchable
    ```
      - curl: curl -XPUT 'localhost:9200/twitter/tweet/1?pretty' -H 'Content-Type: application/json' -d'
          {
              "user" : "kimchy",
              "post_date" : "2009-11-15T14:12:12",
              "message" : "trying out Elasticsearch"
          }
          '
      - kibana console: 
          PUT twitter/tweet/1
          {
              "user" : "kimchy",
              "post_date" : "2009-11-15T14:12:12",
              "message" : "trying out Elasticsearch"
          }
      (example inserts the JSON document into the "twitter" index, under a type called "tweet" with an id of 1)
    ```
