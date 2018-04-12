
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
   - Above query with only lets say specific fields required in output(Note the _source key)
     ```
        curl -XGET 'localhost:9200/cstore/_search?pretty' -H 'Content-Type: application/json' -d'
        {
            "_source": ["field name 1", "field name 2"],
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
   - To get count only
     ```
        curl -XGET 'localhost:9200/cstore/_count?pretty' -H 'Content-Type: application/json' -d'
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
