# terms look up query

```json
// index1
PUT classic_movies/_doc/1
{

  "title": "Jaws",

  "director": "Steven Spielberg"
}

PUT classic_movies/_doc/2
{

  "title": "Jaws II",

  "director": "Jeannot Szwarc"
}

PUT classic_movies/_doc/3
{

  "title": "Ready Player One",

  "director": "Steven Spielberg"
}

// index2
PUT classic_movies_term/_doc/1 
{
    "director_term": "Steven Spielberg"
}
```

```json
GET classic_movies/_search
{
    "query": {
        "terms": {
            "director": {
                "index": "classic_movies_term",
                "id": "1",
                "path": "director_term"
            }
        }
    }
}

// result
[
    {

        "title": "Ready Player One",

        "director": "Steven Spielberg"
    },
    {

        "title": "Jaws",

        "director": "Steven Spielberg"
    }
]
```

The terms lookup query helps build a query based on values obtained from another document rather than a set of values passed in the query. It offers greater flexibility when constructing query terms: we can easily swap the index with any other index from which to obtain a document. For example, say we have an index called movie_search_terms_index containing several documents of search terms (docu- ment 1 contains director terms, document 2 contains actors terms, and so on). We can reference this document with director terms from movie_search_terms_index in our main query and fetch the results. This way, the main query can be constant while the lookup documents are changed as required. Now that we understand terms queries, let’s move on to a query type where we fetch documents given a set of IDs.