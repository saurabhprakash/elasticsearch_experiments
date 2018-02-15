### Basic APIs:
  1. Cluster health: 
  ```
      * kibana console: "GET /_cat/health?v"
      * curl: curl -XGET 'localhost:9200/_cat/health?v&pretty'
  ```
  2. Nodes information: 
  ```
      * kibana console: "GET /_cat/nodes?v"
      * curl: curl -XGET 'localhost:9200/_cat/nodes?v&pretty'
  ```
  3. List all indexes: 
  ```
      * kibana console: "GET /_cat/indices?v"
      * curl: curl -XGET 'localhost:9200/_cat/indices?v&pretty'
  ```
  4. Create an index(and pretty-print the JSON response): 
  ```
      * kibana console: "PUT /customer?pretty"
      * curl: curl -XPUT 'localhost:9200/customer?pretty&pretty'
  ```
  5. Get all index data:
  ```
    http://localhost:9200/<index-name>/_search?pretty=true&q=*:*

    size defaults to 10, so you may also need &size=BIGNUMBER to get more than 10 items. (where BIGNUMBER equals a number you believe is bigger than your dataset)

    BUT, elasticsearch documentation suggests for large result sets, using the scan search type.

    eg:

    curl -XGET 'localhost:9200/<index-name>/_search?search_type=scan&scroll=10m&size=50' -d '
    {
        "query" : {
            "match_all" : {}
        }
    }'
  ```
  6. Adding(Replacing) a document to index:
  ```
      * kibana console: PUT /customer/doc/1?pretty
                          {
                            "name": "John Doe"
                          }
      * curl: curl -XPUT 'localhost:9200/customer/doc/1?pretty&pretty' -H 'Content-Type: application/json' -d'
                            {
                              "name": "John Doe"
                            }
                          '
      * 1 mentioned in above api is id, ID part is optional. If not specified, Elasticsearch will generate a random ID and then use it to index the document. The actual ID Elasticsearch generates (or whatever we specified explicitly in the previous examples) is returned as part of the index API call.
      * Note: It is important to note that Elasticsearch does not require you to explicitly create an index first before you can index documents into it. In the previous example, Elasticsearch will automatically create the customer index if it didn’t already exist beforehand
      * Same api is used to replacing the content of document if t
  ```
  7. Retrieve that document:
  ```
      * kibana console: GET /customer/doc/1?pretty
      * curl: curl -XGET 'localhost:9200/customer/doc/1?pretty&pretty'
  ```
  8. Delete an index:
  ```
      * kibana console: DELETE /customer?pretty
      * curl: curl -XDELETE 'localhost:9200/customer?pretty&pretty'
  ```
  9. Pattern of most of apis: ```"REST Verb /Index/Type/ID"```
  10. Updating documents: In step 5 replacing was remove and creating again a document, for updating a document in place, we can use following:
  ```
    * Normal exiting key updates:
        * kibana console: POST /customer/doc/1/_update?pretty
                          {
                            "doc": { "name": "Jane Doe", "age": 20 }
                          }
        * curl:curl -XPOST 'localhost:9200/customer/doc/1/_update?pretty&pretty' -H 'Content-Type: application/json' -d'
                {
                  "doc": { "name": "Jane Doe" }
                }
                '
    * Add a key in update document:
      * kibana console: POST /customer/doc/1/_update?pretty
                        {
                          "script" : "ctx._source.age += 5"
                        }
      * curl: curl -XPOST 'localhost:9200/customer/doc/1/_update?pretty&pretty' -H 'Content-Type: application/json' -d'
                        {
                          "doc": { "name": "Jane Doe", "age": 20 }
                        }
                        '
     * Updates by script:
        * kibana console: POST /customer/doc/1/_update?pretty
                          {
                            "script" : "ctx._source.age += 5"
                          }
        * curl: curl -XPOST 'localhost:9200/customer/doc/1/_update?pretty&pretty' -H 'Content-Type: application/json' -d'
                {
                  "script" : "ctx._source.age += 5"
                }
                ' 
  ```
  11. Deleting a document:
      ```
        * kibana console: DELETE /customer/doc/2?pretty
        * curl: curl -XDELETE 'localhost:9200/customer/doc/2?pretty&pretty'
      ```
  12. Batch processing: Ability to index, update, and delete in batch:
      ```
      * Add:
        * kibana console: POST /customer/doc/_bulk?pretty
            {"index":{"_id":"1"}}
            {"name": "John Doe" }
            {"index":{"_id":"2"}}
            {"name": "Jane Doe" }
        * curl: 
          curl -XPOST 'localhost:9200/customer/doc/_bulk?pretty&pretty' -H 'Content-Type: application/json' -d'
            {"index":{"_id":"1"}}
            {"name": "John Doe" }
            {"index":{"_id":"2"}}
            {"name": "Jane Doe" }
            '
       * Update:
         * Kibana console: POST /customer/doc/_bulk?pretty
            {"update":{"_id":"1"}}
            {"doc": { "name": "John Doe becomes Jane Doe" } }
            {"delete":{"_id":"2"}}
         * curl: curl -XPOST 'localhost:9200/customer/doc/_bulk?pretty&pretty' -H 'Content-Type: application/json' -d'
            {"update":{"_id":"1"}}
            {"doc": { "name": "John Doe becomes Jane Doe" } }
            {"delete":{"_id":"2"}}
            '
      ```





