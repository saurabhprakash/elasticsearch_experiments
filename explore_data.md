1. Load data from accounts.json
    ```
    curl -H "Content-Type: application/json" -XPOST 'localhost:9200/bank/account/_bulk?pretty&refresh' --data-binary "@accounts.json"
    curl 'localhost:9200/_cat/indices?v'
    ```
2. Search - REST request URI
    ```
    kibana console: GET /bank/_search?q=*&sort=account_number:asc&pretty
    curl: curl -XGET 'localhost:9200/bank/_search?q=*&sort=account_number:asc&pretty&pretty'
    ```
3. Search - Request body method
    ```
    kibana console:
        GET /bank/_search
        {
        "query": { "match_all": {} },
        "sort": [
            { "account_number": "asc" }
        ]
        }
    curl:
        curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
        {
        "query": { "match_all": {} },
        "sort": [
            { "account_number": "asc" }
        ]
        }
        '
    ```
4. Search pagination
    ```
    kibana console:
        GET /bank/_search
        {
        "query": { "match_all": {} },
        "from": 10,
        "size": 10
        }
    curl:
        curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
        {
        "query": { "match_all": {} },
        "from": 10,
        "size": 10
        }
        '
    ```
5. Return specific fields in search( return two fields, account_number and balance):
    ```
    kibana console:
        GET /bank/_search
        {
        "query": { "match_all": {} },
        "_source": ["account_number", "balance"]
        }
    curl:
        curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
        {
        "query": { "match_all": {} },
        "_source": ["account_number", "balance"]
        }
        '
    ```
6. match query
    ```
    kibana console:
        GET /bank/_search
        {
        "query": { "match": { "account_number": 20 } }
        }
        curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
        {
        "query": { "match": { "account_number": 20 } }
        }
        '

        all accounts containing the term "mill" in the address:
        GET /bank/_search
        {
        "query": { "match": { "address": "mill" } }
        }
        curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
        {
        "query": { "match": { "address": "mill" } }
        }
        '

        all accounts containing the term "mill" or "lane" in the address:

        GET /bank/_search
        {
        "query": { "match": { "address": "mill lane" } }
        }
        curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
        {
        "query": { "match": { "address": "mill lane" } }
        }
        '

        all accounts containing the phrase "mill lane" in the address:

        GET /bank/_search
        {
        "query": { "match_phrase": { "address": "mill lane" } }
        }
        GET /bank/_search
        {
        "query": { "match_phrase": { "address": "mill lane" } }
        }
    ```
7. Bool Query
    ```
        all accounts containing "mill" and "lane" in the address:
        kibana console:  GET /bank/_search
            {
            "query": {
                "bool": {
                "must": [
                    { "match": { "address": "mill" } },
                    { "match": { "address": "lane" } }
                ]
                }
            }
            }
        curl: curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
            {
            "query": {
                "bool": {
                "must": [
                    { "match": { "address": "mill" } },
                    { "match": { "address": "lane" } }
                ]
                }
            }
            }
            '
        
        returns all accounts containing "mill" or "lane" in the address:

        kibana console: GET /bank/_search
        {
        "query": {
            "bool": {
            "should": [
                { "match": { "address": "mill" } },
                { "match": { "address": "lane" } }
            ]
            }
        }
        }
        curl: curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
        {
        "query": {
            "bool": {
            "should": [
                { "match": { "address": "mill" } },
                { "match": { "address": "lane" } }
            ]
            }
        }
        }
        '

        returns all accounts that contain neither "mill" nor "lane" in the address:

        kibana console: GET /bank/_search
        {
        "query": {
            "bool": {
            "must_not": [
                { "match": { "address": "mill" } },
                { "match": { "address": "lane" } }
            ]
            }
        }
        }
        curl: curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
        {
        "query": {
            "bool": {
            "must_not": [
                { "match": { "address": "mill" } },
                { "match": { "address": "lane" } }
            ]
            }
        }
        }
        '

        multi-level boolean logic, returns all accounts of anybody who is 40 years old but doesn’t live in ID(aho):

        kibana console: GET /bank/_search
        {
        "query": {
            "bool": {
            "must": [
                { "match": { "age": "40" } }
            ],
            "must_not": [
                { "match": { "state": "ID" } }
            ]
            }
        }
        }
        curl: curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
        {
        "query": {
            "bool": {
            "must": [
                { "match": { "age": "40" } }
            ],
            "must_not": [
                { "match": { "state": "ID" } }
            ]
            }
        }
        }
        '
    ```
  8. Group By and Aggregations
        * Only group by
            ```
                a. kibana console: GET /bank/_search
                {
                  "size": 0,
                  "aggs": {
                    "group_by_state": {
                      "terms": {
                        "field": "state.keyword"
                      }
                    }
                  }
                }
                b. curl: curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
                    {
                      "size": 0,
                      "aggs": {
                        "group_by_state": {
                          "terms": {
                            "field": "state.keyword"
                          }
                        }
                      }
                    }
                    '
            ```
        * Group by and average aggregration:
            ```
                kibana console: 
                    GET /bank/_search
                    {
                      "size": 0,
                      "aggs": {
                        "group_by_state": {
                          "terms": {
                            "field": "state.keyword"
                          },
                          "aggs": {
                            "average_balance": {
                              "avg": {
                                "field": "balance"
                              }
                            }
                          }
                        }
                      }
                    }
                curl: 
                    curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
                    {
                      "size": 0,
                      "aggs": {
                        "group_by_state": {
                          "terms": {
                            "field": "state.keyword"
                          },
                          "aggs": {
                            "average_balance": {
                              "avg": {
                                "field": "balance"
                              }
                            }
                          }
                        }
                      }
                    }
                    '
            ```
        * Nested groupy and aggregations
            ```
                a. kinana console:
                    GET /bank/_search
                        {
                          "size": 0,
                          "aggs": {
                            "group_by_age": {
                              "range": {
                                "field": "age",
                                "ranges": [
                                  {
                                    "from": 20,
                                    "to": 30
                                  },
                                  {
                                    "from": 30,
                                    "to": 40
                                  },
                                  {
                                    "from": 40,
                                    "to": 50
                                  }
                                ]
                              },
                              "aggs": {
                                "group_by_gender": {
                                  "terms": {
                                    "field": "gender.keyword"
                                  },
                                  "aggs": {
                                    "average_balance": {
                                      "avg": {
                                        "field": "balance"
                                      }
                                    }
                                  }
                                }
                              }
                            }
                          }
                        }
                b. curl:
                    curl -XGET 'localhost:9200/bank/_search?pretty' -H 'Content-Type: application/json' -d'
                    {
                      "size": 0,
                      "aggs": {
                        "group_by_age": {
                          "range": {
                            "field": "age",
                            "ranges": [
                              {
                                "from": 20,
                                "to": 30
                              },
                              {
                                "from": 30,
                                "to": 40
                              },
                              {
                                "from": 40,
                                "to": 50
                              }
                            ]
                          },
                          "aggs": {
                            "group_by_gender": {
                              "terms": {
                                "field": "gender.keyword"
                              },
                              "aggs": {
                                "average_balance": {
                                  "avg": {
                                    "field": "balance"
                                  }
                                }
                              }
                            }
                          }
                        }
                      }
                    }
                    '
            ```
