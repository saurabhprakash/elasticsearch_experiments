


1. All documents across all types within the twitter index
    ```
      GET /twitter/_search?q=user:kimchy
    ```
2. Search within specific types
    ```
      GET /twitter/tweet,user/_search?q=user:kimchy
    ```
3. Search all tweets with a certain tag across several indices (for example, when each user has his own index)
    ```
      GET /kimchy,elasticsearch/tweet/_search?q=user:kimchy
    ```
4. Search all tweets across all available indices using _all placeholder
    ```
      GET /_all/tweet/_search?q=user:kimchy
    ```
5. Search across all indices and all types
    ```
      GET /_search?q=user:kimchy
    ```
