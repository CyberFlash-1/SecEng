## Create Index Mappings 

POST: Add a document to an index 
PUT: create an index 
GET: perform a search on that index

1. Send a request that indexes the following sample document and create a new index called sample_blog:
```
POST sample_blog/_doc
{
  "@timestamp": "2021-03-10T16:00:00.000Z",
  "abstract": "The Joy of Painting",
  "author": "Bob Ross",
  "body": "Painting should do one thing. It should put happiness in your heart. We'll take a little bit of Van Dyke Brown. Isn't that fantastic? You can just push a little tree out of your brush like that. Mix your color marbly don't mix it dead.",
  "body_word_count": 55,
  "category": "Painting",
  "title": "Making Happy Little Trees",
  "url": "/blog/happy-little-trees",
  "published": true
}
```

QUERY RESPONSE:
```
{
  "_index": "sample_blog",
  "_id": "VHtFsqABWkilohjLLQ08",
  "_version": 1,
  "result": "created",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 0,
  "_primary_term": 1
}
```

<div align ="center">
<img width="900" height="300" alt="image" src="https://github.com/user-attachments/assets/e6bf7231-8e5c-4414-b11a-546693e73134" />
</div>

---

2. Now let's make some changes to the blogs index. First, create a new blogs_fixed index.

```
PUT /blogs_fixed
```
QUERY RESPONSE:
```
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "blogs_fixed"
}
```
3. Reindex all of the documents from the blogs index into your new blogs_fixed index.

```
POST _reindex
{
  "source": {
    "index": "blogs"
  },
  "dest": {
    "index": "blogs_fixed"
  }
}
```
QUERY RESPONSE:
```
{
  "took": 2661,
  "timed_out": false,
  "total": 3687,
  "updated": 3687,
  "created": 0,
  "deleted": 0,
  "batches": 4,
  "version_conflicts": 0,
  "noops": 0,
  "retries": {
    "bulk": 0,
    "search": 0
  },
  "throttled_millis": 0,
  "requests_per_second": -1,
  "throttled_until_millis": 0,
  "failures": []
}
```

4. Run two queries for kim in the authors.first_name field, one for the blogs index and the other for the blogs_fixed index. Confirm the changes were successful by comparing both requests.

```
GET blogs/_search
{
  "query": {
    "match": {
      "authors.first_name": "kim"
    }

GET blogs_fixed/_search
{
  "query": {
    "match": {
      "authors.first_name": "kim"
    }
  }
}    
  }
}
```
