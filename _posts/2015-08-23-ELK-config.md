---
# layout: post
title: ELK configuration
---

ELK means ElasticSearch + Logstash + Kibana. These products could provide a convenient way to analyze all kinds of logs.

This post record the way I use them. Here is the versions in my environment.

- ElasticSearch 1.6.0
+ Logstash 1.5.3
- Kibana 4.1.1

### ElasticSearch Config
Here is an checklist for [ElasticSearch optimization](https://github.com/garyelephant/blog/blob/master/elasticsearch_optimization_checklist.md)

Following is the configuration for ElasticSearch's template. Run `curl -XPOST http://localhost:9200/_template/logstash -d '%FOLLOWING_JSON_CONTENT%'` to create a index template.

~~~
{
  "order": 0,
  "template": "logstash-*",
  "settings": {
    "index.refresh_interval": "5s"
  },
  "mappings": {
    "_default_": {
      "dynamic_templates": [
        {
          "message_field": {
            "mapping": {
              "index": "not_analyzed",
              "type": "string",
              "doc_values": true
            },
            "match_mapping_type": "string",
            "match": "message"
          }
        },
        {
          "string_fields": {
            "mapping": {
              "index": "not_analyzed",
              "type": "string",
              "doc_values": true
            },
            "match_mapping_type": "string",
            "match": "*"
          }
        },
        {
          "long_fields": {
            "mapping": {
              "index": "not_analyzed",
              "type": "long",
              "doc_values": true
            },
            "match_mapping_type": "long",
            "match": "*"
          }
        },
        {
          "double_fields": {
            "mapping": {
              "index": "not_analyzed",
              "type": "double",
              "doc_values": true
            },
            "match_mapping_type": "double",
            "match": "*"
          }
        },
        {
          "object_fields": {
            "mapping": {
              "index": "not_analyzed",
              "type": "object",
              "doc_values": true
            },
            "match_mapping_type": "object",
            "match": "*"
          }
        }
      ],
      "_all": {
        "omit_norms": true,
        "enabled": true
      },
      "_source": {
        "compress": true
      },
      "properties": {
        "geoip": {
          "dynamic": true,
          "type": "object",
          "properties": {
            "location": {
              "type": "geo_point"
            }
          }
        },
        "@version": {
          "index": "not_analyzed",
          "type": "string"
        }
      }
    }
  },
  "aliases": {}
}
~~~

### Logstash config
Following is an example configuration for our nginx logs.

~~~
input {
    file {
        path => "/data/logs/nginx/access.log"
        start_position => "beginning"
    }
#    stdin{}
}
filter {
    grok {
        match => { "message" => "%{HOSTNAME:requestHost} - %{IPV4:requestIp} - %{NOTSPACE:requestRemoteUser} \[%{HTTPDATE:requestHttpDate}\] %{QS:requestUrl} %{NUMBER:requestHttpStatus:int} %{NUMBER:requestBodySentBytes:int} %{QS:requestHttpReferer} %{QS:requestHttpUserAgent} %{QS:requestHttpXForwaredFor} \"%{NUMBER:requestTime:float}\""}
    }
    date {
        match => ["requestHttpDate", "dd/MMM/yyyy:HH:mm:ss Z"]
    }
    ### extract key/value pairs from url.
    kv {
        source => "requestUrl"
        field_split => "&? "
        trimkey => "_"
        trim => " "
    }
    mutate {
        convert => {
            "launch" => "integer"
            "screenDpi" => "float"
            "screenWidth" => "integer"
            "screenHeight" => "integer"
            "longitude" => "float"
            "latitude" => "float"
        }
    }
    urldecode {
        all_fields => true
    }
    output {
        elasticsearch {
            protocol => "transport"
            cluster => "mc-es-ai"
            index => "logstash-busybox-%{+MM.dd}"
            host => ["xxx"]
        }
#        stdout {
#            codec => "json"
#        }
    }
}
~~~

### Performance Enhancement
We could enhance the performance by changing some of the properties in template.

~~~
index.store.type: mmapfs
indices.memory.index_buffer_size: 30%
index.translog.flush_threshold_ops: 50000
refresh_interval: 1m
~~~

Changing some of the properties in cluster settings could boost the performance too.

~~~
PUT /_cluster/settings
{
    "persistent" : {
        "indices.store.throttle.max_bytes_per_sec" : "100mb"
    }
}
If you are doing a bulk import and don’t care about search at all, you can disable merge throttling entirely. This will allow indexing to run as fast as your disks will allow:

PUT /_cluster/settings
{
    "transient" : {
        "indices.store.throttle.type" : "none"
    }
}
Setting the throttle type to none disables merge throttling entirely. When you are done importing, set it back to merge to reenable throttling.

PUT /_cluster/settings
{
    "persistent" : {
        "threadpool.index.type" : "fixed"
        "threadpool.index.queue_size": 1000
    }
}
Setting the index threadpool queue size larger. The default queue size is 200.
~~~
