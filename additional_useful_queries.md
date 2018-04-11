
 - Single field exact query: 
    ```
        curl -XGET "http://localhost:9200/<index name>/_search" -H 'Content-Type: application/json' -d'
        {
            "query" : {
                "term" : { "<field name>" : "<value>"}
            }
        }'
     ```
  - Multifield exact query:
     ```
        curl -XGET 'localhost:9200/<index name>/_search?pretty' -H 'Content-Type: application/json' -d'
        {
            "query": {
                "constant_score" : {
                    "filter" : {
                         "bool" : {
                            "must" : [
                                { "term" : { "<field name 1>" : "<value 1>" } },
                                { "term" : { "<field name 2>" : "<value 2>" } }
                            ]
                        }
                    }
                }
            }
        }'
     ```
