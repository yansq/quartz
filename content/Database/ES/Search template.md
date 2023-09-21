# Search Template

## Create  templates

```json
PUT _scripts/my-search-template
{
  "script": {
    "lang": "mustache",
    "source": {
      "query": {
        "match": {
          "message": "{{query_string}}"
        }
      },
      "from": "{{from}}",
      "size": "{{size}}"
    },
    "params": {
      "query_string": "My query string"
    }
  }
}
```

## Validate templates

```json
POST _render/template
{
  "id": "my-search-template",
  "params": {
    "query_string": "hello world",
    "from": 20,
    "size": 10
  }
}
```

When executing, will show the searching request body:
```json
{
  "template_output": {
    "query": {
      "match": {
        "message": "hello world"
      }
    },
    "from": "20",
    "size": "10"
  }
}
```

## Search  by templates

```json
GET my-index/_search/template
{
  "id": "my-search-template",
  "params": {
    "query_string": "hello world",
    "from": 0,
    "size": 10
  }
}
```

## Get all search templates 

```json
GET _cluster/state/metadata?pretty&filter_path=metadata.stored_scripts
```
