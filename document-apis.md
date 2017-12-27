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
  * Automatic id generation
    ```
      - curl: curl -XPOST 'localhost:9200/twitter/tweet/?pretty' -H 'Content-Type: application/json' -d'
              {
                  "user" : "kimchy",
                  "post_date" : "2009-11-15T14:12:12",
                  "message" : "trying out Elasticsearch"
              }
              '
      - kibana console: 
          POST twitter/tweet/
          {
              "user" : "kimchy",
              "post_date" : "2009-11-15T14:12:12",
              "message" : "trying out Elasticsearch"
          }
    ```
### GET APIs (https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-get.html)
The get API allows to get a typed JSON document from the index based on its id. By default, the get API is realtime, and is not affected by the refresh rate of the index (when data will become visible for search).

Additional Parms which can be passed during this operation:

  * _source:By default, the get operation returns the contents of the _source field unless you have used the stored_fields parameter or if the _source field is disabled. You can turn off _source retrieval by using the _source parameter
  * stored_fields: The get operation allows specifying a set of stored fields that will be returned by passing the stored_fields parameter. If the requested fields are not stored, they will be ignored. 
  * refresh: The refresh parameter can be set to true in order to refresh the relevant shard before the get operation and make it searchable. Setting it to true should be done after careful thought and verification that this does not cause a heavy load on the system (and slows down indexing).
  * version: You can use the version parameter to retrieve the document only if its current version is equal to the specified one. This behavior is the same for all version types with the exception of version type FORCE which always retrieves the document. Note that FORCE version type is deprecated.

### Delete API (https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-delete.html)
The delete API allows to delete a typed JSON document from a specific index based on its id. The following example deletes the JSON document from an index called twitter, under a type called tweet, with id valued 1

### Delete by query (https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-delete-by-query.html)
The simplest usage of _delete_by_query just performs a deletion on every document that match a query. It’s also possible to delete documents of multiple indexes and multiple types at once, just like the search API. In addition to the standard parameters like pretty, the Delete By Query API also supports refresh, wait_for_completion, wait_for_active_shards, and timeout. Sending the refresh will refresh all shards involved in the delete by query once the request completes. 

### Update API
The update API allows to update a document based on a script provided. The operation gets the document (collocated with the shard) from the index, runs the script (with optional script language and parameters), and index back the result (also allows to delete, or ignore the operation). It uses versioning to make sure no updates have happened during the "get" and "reindex".
Upserts
If the document does not already exist, the contents of the upsert element will be inserted as a new document. If the document does exist, then the script will be executed instead.

 ```
   Kibana console sample operations
   Sample data for below operations
      PUT test/type1/1
      {
          "counter" : 1,
          "tags" : ["red", "blue"]
      }
 
   1. Update exiting field
       POST test/type1/1/_update
        {
            "script" : {
                "source": "ctx._source.counter += params.count",
                "lang": "painless",
                "params" : {
                    "count" : 4
                }
            }
        }
   2. Add a new field
        POST test/type1/1/_update
        {
            "script" : "ctx._source.new_field = 'value_of_new_field'"
        }
   3. Remove a field
        POST test/type1/1/_update
        {
            "script" : "ctx._source.remove('new_field')"
        }
 ```
