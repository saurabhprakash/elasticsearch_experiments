### Basic APIs:
  1. Cluster health: 
      * kibana console: "GET /_cat/health?v"
      * curl: curl -XGET 'localhost:9200/_cat/health?v&pretty'
  2. Nodes information: 
      * kibana console: "GET /_cat/nodes?v"
      * curl: curl -XGET 'localhost:9200/_cat/nodes?v&pretty'
  3. List all indexes: 
      * kibana console: "GET /_cat/indices?v"
      * curl: curl -XGET 'localhost:9200/_cat/indices?v&pretty'
  4. Create an index(and pretty-print the JSON response): 
      * kibana console: "PUT /customer?pretty"
      * curl: curl -XPUT 'localhost:9200/customer?pretty&pretty'
  5. Adding a document to index:
      * kibana console: PUT /customer/doc/1?pretty
                          {
                            "name": "John Doe"
                          }
         (Note: It is important to note that Elasticsearch does not require you to explicitly create an index first before you can index documents into it. In the previous example, Elasticsearch will automatically create the customer index if it didn’t already exist beforehand)
      * curl: curl -XPUT 'localhost:9200/customer/doc/1?pretty&pretty' -H 'Content-Type: application/json' -d'
                            {
                              "name": "John Doe"
                            }
                          '
  6. Retrieve that document:
      * kibana console: GET /customer/doc/1?pretty
      * curl: curl -XGET 'localhost:9200/customer/doc/1?pretty&pretty'
  7. Delete an index:
      * kibana console: DELETE /customer?pretty
      * curl: curl -XDELETE 'localhost:9200/customer?pretty&pretty'
  8. Pattern of most of apis: "REST Verb /Index/Type/ID"
      






