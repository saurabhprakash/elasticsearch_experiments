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
  * Index operation also accepts an op_type that can be used to force a create operation, allowing for "put-if-absent" behavior. When create is used, the index operation will fail if a document by that id already exists in the index
    ```
      - curl (method 1): curl -XPUT 'localhost:9200/twitter/tweet/1?op_type=create&pretty' -H 'Content-Type: application/json' -d'
        {
            "user" : "kimchy",
            "post_date" : "2009-11-15T14:12:12",
            "message" : "trying out Elasticsearch"
        }
       '
      - kibana console (method 1): 
         PUT twitter/tweet/1?op_type=create
           {
               "user" : "kimchy",
               "post_date" : "2009-11-15T14:12:12",
               "message" : "trying out Elasticsearch"
           } 
      - curl (method 2): curl -XPUT 'localhost:9200/twitter/tweet/1/_create?pretty' -H 'Content-Type: application/json' -d'
        {
            "user" : "kimchy",
            "post_date" : "2009-11-15T14:12:12",
            "message" : "trying out Elasticsearch"
        }
        '
      - kibana console(method 2):
          PUT twitter/tweet/1/_create
          {
              "user" : "kimchy",
              "post_date" : "2009-11-15T14:12:12",
              "message" : "trying out Elasticsearch"
          } 
    ```
