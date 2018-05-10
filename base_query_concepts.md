

- Paginating results using from and size: ```curl 'localhost:9200/get-together/_search?from=10&size=10'``` [ search request will search for all events, but you want the second page of 10 items]
- Changing the order of the results: ```curl 'localhost:9200/get-together/_search?sort=date:asc'``` [Request matching all documents but returning the default first 10 of all results ordered by date in ascending order]
- Limiting the fields from source that you want in the response: ```curl 'localhost:9200/get-together/_search?sort=date:asc&_source=title,date'``` [Request matching all documents but return the default first 10 of all results ordered by date in ascending order. You want only two fields in the response: title and date]
- URL-based search request where you want to return only documents con- taining the word “elasticsearch” in the title: ```curl 'localhost:9200/get-together/_search?sort=date:asc&q=title:elasticsearch'```
