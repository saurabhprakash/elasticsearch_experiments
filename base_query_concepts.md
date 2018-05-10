##### Url based search request

- Paginating results using from and size: ```curl 'localhost:9200/get-together/_search?from=10&size=10'``` [ search request will search for all events, but you want the second page of 10 items]
- Changing the order of the results: ```curl 'localhost:9200/get-together/_search?sort=date:asc'``` [Request matching all documents but returning the default first 10 of all results ordered by date in ascending order]
- Limiting the fields from source that you want in the response: ```curl 'localhost:9200/get-together/_search?sort=date:asc&_source=title,date'``` [Request matching all documents but return the default first 10 of all results ordered by date in ascending order. You want only two fields in the response: title and date]
- URL-based search request where you want to return only documents con- taining the word “elasticsearch” in the title: ```curl 'localhost:9200/get-together/_search?sort=date:asc&q=title:elasticsearch'```

##### Request body–based search request: 

    1. listing searches for the second page of the index when all documents are matched
        curl 'localhost:9200/get-together/_search' -d '{
            "query": {
                "match_all": {}
            },
            "from": 10,
            "size": 10
        }'

    2. Following listing, returning the name and date fields of each matching group.
        curl 'localhost:9200/get-together/_search' -d '{
            "query": {
                "match_all": {}
            },
            "_source": ["name", "date"]
        }'
        
    3. WILDCARDS IN RETURNED FIELDS WITH _SOURCE: We can also specify multiple wildcards using an array of wildcard strings, like _source: ["name.*", "address.*"], Filtering the returned _source showing include and exclude
        curl 'localhost:9200/get-together/_search' -d '{
          "query": {
        "match_all": {}
        },
        "_source": {
            "include": ["location.*", "date"],
            "exclude": ["location.geolocation"]
        } }'

        [On above: Return fields starting with location and date fields with the search response, Don’t return the field location.geolocation]
        
    4. SORT ORDER FOR RESULTS: If no sort order is specified, Elasticsearch returns matching documents sorted by the _score value in descending order, with the most relevant (highest scoring) documents first. Below see, Results sorted by date (ascending), name (descending), and _score
        curl 'localhost:9200/get-together/_search' -d '{
            "query": {
                "match_all": {}
            },
            "sort": [
                {"created_on": "asc"},
                {"name": "desc"},
                "_score"
        ] }'
