
 - Single field exact query: 
    ```
        curl -XGET "http://localhost:9
        200/<index name>/_search?pretty" -H 'Content-Type: application/json' -d'
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
        curl -XGET 'localhost:9200/<index-name>/_search?pretty' -H 'Content-Type: application/json' -d'
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
        curl -XGET 'localhost:9200/<index-name>/_count?pretty' -H 'Content-Type: application/json' -d'
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
   - For error "Fielddata is disabled on text fields by default" (More read on this: https://www.elastic.co/guide/en/elasticsearch/reference/current/fielddata.html#_fielddata_is_disabled_on_literal_text_literal_fields_by_default)
     ```
        curl request: curl -XPUT "http://localhost:9200/<index name>/_mapping/<document type name>" -H 'Content-Type: application/json' -d'
              {
                "properties": {
                  "<field name>": { 
                    "type":     "text",
                    "fielddata": true
                  }
                }
              }'
              
         kibana: 
         PUT <index name>/_mapping/<document type name>
         {
           "properties": {
             "<field name>": { 
               "type":     "text",
               "fielddata": true
             }
           }
         }
     ```
   - Getting all the distinct values for a given field in an index:
     ```
          kibana console:
             GET <index_name>/_search
             {
               "size": 0,
               "aggs": {
                 "distinct_CSPAccountNo": {
                   "terms": {
                     "field": "CSPAccountNo",
                     "size": 1000
                   }
                 }
               }
             }
          
          curl request:
              curl -XGET "http://localhost:9200/<index name>/_search" -H 'Content-Type: application/json' -d'
              {
                "size": 0,
                "aggs": {
                  "distinct_<field_name>": {
                    "terms": {
                      "field": "<field_name>",
                      "size": 1000
                    }
                  }
                }
              }'
     ```
  - Remove default fields returned by elasticsearch
     ```
       curl -XGET 'localhost:9200/<index_name>/_search?pretty&filter_path=hits.hits._source' -H 'Content-Type: application/json' -d'
       {
           "_source": [<"field_names">],
           "query": {
               "constant_score" : {
                   "filter" : {
                       "bool" : {
                           "must" : [
                               { "term" : { <field_name_to_query> : <value_to_query> } }
                           ]
                       }
                   }
               }
           }
       }'
     ```
     
  - Max results return by ES is by default 10000, to increase that use this command:
  ```
  curl -XPUT "http://localhost:9200/<index_name>/_settings"  -H 'Content-Type: application/json' -d '{ "index" : { "max_result_window" : <INT size to return> } }'
  ```
