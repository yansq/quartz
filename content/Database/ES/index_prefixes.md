# index_prefixes

```json
PUT boxoffice_hit_movies
{
    "mappings": {
        "properties": {
            "title": {
                "type": "text",
                "index_prefixes": {}
            }
        }
    }
}
```

The sole `title` property includes an additional property, `index_prefixes`. This indicates to the engine that during the indexing process, it should create the field with prebuilt prefixes and store those values. It can speed up prefix queries.