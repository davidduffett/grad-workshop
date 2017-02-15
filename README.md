# Search at SEEK - Graduate Workshop - 2017

For convenience we have setup an Elasticsearch cluster for you to play with. But if you want to setup a cluster in your private machine or your work laptop on your own time, following are some useful links.

* [Elasticsearch Download] (https://www.elastic.co/downloads/elasticsearch)
* [Elasticsearch: The Definitive Guide] (https://www.elastic.co/guide/en/elasticsearch/guide/current/index.html)
* [Kibana: Analytics/Visualization Tool] (https://www.elastic.co/products/kibana)

## Exerices

### Create the Index

In order to perform searches, your documents needs to be in an Elasticsearch index. Lets create one. For this workshop purpose, lets assume we are indexing job ads and a job has the following attributes.

* id - A unique identifier
* title - Title of the job ad
* description - The job description
* advertiser - Advertiser of the job ad
* publishedDate - Date the job ad was published

Go to Elasticsearch URL and click on "Dev Tools". This will take you to "Console" which is an interactive tool that you can use to run commands against Elasticsearch. 

Copy paste the follwong snippet into Console. Make sure to replace the text ```{your surname}``` with your surname so that you won't overwrite each others work :-).  And then click the Play button (Green button).

```
PUT jobs-{your surname}
{
    "settings": {
        "index": {
            "number_of_shards": 3, 
            "number_of_replicas": 2 
        }
    },
    "mappings": {
        "job": {
            "properties": {
                "title": {
                  "type": "text",
                  "index": "analyzed"
                },
                "description": {
                  "type": "text",
                  "index": "analyzed"
                },
                "advertiser": {
                  "properties": {
                    "id": {
                      "type": "long"
                    },
                    "name": {
                      "type": "text",
                      "fields": {
                        "keyword": {
                          "type": "keyword",
                          "ignore_above": 256
                        }
                      }
                    }
                  }
                },
                "publishedDate": {
                  "type" : "date",
                  "format": "date"
                }
            }
        }
    }
}
```

The above snippet will create an index named '''jobs'''. We can now start indexing documents. If you want to learn more about creating indexes refer to [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html).

### Index documents

Copy paste the following snippets to Console and run them one by one, feel free to change text if you like. Make sure to replace ```{your name}``` with your surname.

```
PUT /jobs-{your surname}/job/1
{
  "title": "product manager",
  "description": "lets build awesome products",
  "advertiser": {
    "name": "abc recruitment",
    "id": 1
  },
  "publishedDate": "2017-02-15"
}
```

```
PUT /jobs-{your surname}/job/2
{
  "title": "business analyst",
  "description": "analyse business, agile, scrum",
  "advertiser": {
    "name": "talent solutions",
    "id": 2
  },
  "publishedDate": "2017-02-13"
}
```

```
PUT /jobs-{your surname}/job/3
{
  "title": "agile tester",
  "description": "test software, report bugs",
  "advertiser": {
    "name": "commonwealth bank",
    "id": 3
  },
  "publishedDate": "2017-02-11"
}
```

```
PUT /jobs-{your surname}/job/4
{
  "title": "agile product manager",
  "description": "needs a product manager with agile experience",
  "advertiser": {
    "name": "abc recruitment",
    "id": 1
  },
  "publishedDate": "2017-02-15"
}
```

```
PUT /jobs-{your surname}/job/5
{
  "title": "delivery manager",
  "description": "needs a delivery manager with agile experience, good sense of humour is a must",
  "advertiser": {
    "name": "talent solutions",
    "id": 2
  },
  "publishedDate": "2017-02-15"
}
```

```
PUT /jobs-{your surname}/job/6
{
  "title": "senior developer",
  "description": "java, go, javascript, html",
  "advertiser": {
    "name": "abc recruitment",
    "id": 1
  },
  "publishedDate": "2017-02-15"
}
```


### Search

Since you have indexed some jobs, you could perform searches and get results/insights.

Find a job by id

```
GET /jobs-niv/job/3
```

Find all jobs that contains `manager` in `title`

```
GET /jobs-niv/job/_search
{
  "query": {
      "match" : {
          "title" : "manager"
      }
  }
}
```

Find jobs that contain words `product` and `manager`

```
GET /jobs-niv/job/_search
{
  "query": {
      "match" : {
          "title" : {
            "query": "product manager",
            "operator": "and"
          }
      }
  }
}

```

Find jobs that are from advertiser with id 1 and contains the word `manager`

```
GET /jobs-niv/job/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "title": "manager"
          }
        },
        {
          "term": {
            "advertiser.id": 1
          }
        }
      ]
    }
  }
}
```
