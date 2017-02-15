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
    "settings" : {
        "index" : {
            "number_of_shards" : 3, 
            "number_of_replicas" : 2 
        }
    },
    "mappings" : {
        "job" : {
            "properties" : {
                "id": { 
                  "type" : "integer",
                  "index": "not_analyzed"
                },
                "title": {
                  "type" : "text",
                  "index" : "analyzed"
                }
            }
        }
    }
}
