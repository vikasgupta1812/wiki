Open Source distributed search platform ELK (Elasticsearch + Logstash + Kibana) Started Learning Resources Index

Bloggers near four-month search hundreds of content, the selection of the following useful English slides and blogs to share out, including distributed indexing and search service Elasticsearch, logstash, data visualization service Kibana learning resources, can greatly ELK reduce the time cost of entry:



1.ELK overall presentation

    (Must see) Using elasticsearch, logstash & kibana to create realtime dashboards
        https://speakerdeck.com/elasticsearch/using-elasticsearch-logstash-and-kibana-to-create-realtime-dashboards
        Elasticsearch official slide

    (Must see) Collect & visualize your logs with Logstash, Elasticsearch & Redis
        http://michael.bouvy.net/blog/en/2013/11/19/collect-visualize-your-logs-logstash-elasticsearch-redis-kibana/
        ELK platform to build specific operations introduced in detail.

    (Must see) 2014 SCALE12X-Introduction to Elasticsearch, Logstash and Kibana
        https://speakerdeck.com/elasticsearch/scale-12x-introduction-to-elasticsearch-logstash-and-kibana
        Elasticsearch explanation accounts for 50%, logstash explanation accounts for 40%, Kibana description 10%

    Use logstash + elasticsearch + kibana quickly build log platform
        http://www.cnblogs.com/buzzlight/p/logstash_elasticsearch_kibana_log.html

    Kibana + Logstash + Elasticsearch log query system
        http://enable.blog.51cto.com/747951/1049411

    Log Analytics Using Elasticsearch-Logstash-Kibana (Part 1)
        http://blog.nugrahais.me/blog/2013/12/23/log-analytics-using-elasticsearch-logstash-kibana-part-1/

    Using ElasticSearch and Logstash to Serve Billions of Searchable Events for Customers
        http://www.elasticsearch.org/blog/using-elasticsearch-and-logstash-to-serve-billions-of-searchable-events-for-customers/
        ELK use a case presentation





2.logstash Introduction

    (Must see) Logstash -Nicolas Szalay
        http://slides.com/aurelienrougemont/logstash/

    (Must see) Getting started with Logstash - New to Logstash Start here?!
        http://logstash.net/docs/1.4.0/tutorials/getting-started-with-logstash
        
    (Must see) Logstash-Jordan Sissel
        http://semicomplete.com/presentations/logstash-scale11x/#/

    Logstash and friends
        http://www.slideshare.net/roidelapluie/logstash-and-friends?qid=0c61ce8f-1a87-4678-a9c7-61a18ae74993&v=default&b=&from_search=11

    Logstash
        http://www.slideshare.net/startit/logstash-29012201?qid=0c61ce8f-1a87-4678-a9c7-61a18ae74993&v=default&b=&from_search=7
        page 11 to page 32: intuitive lists some input, filter, output, explain a bit grok pattern

    Starting out with grok in Logstash
        http://antonlindstrom.com/2012/09/24/starting-out-with-grok-in-logstash.html
        Introduction Logstash Grok Filter

    (Important) Logstash1.4.0 Grok Filter Docs
        http://logstash.net/docs/1.4.0/filters/grok

    (Important) Logstash1.4.0 Grok Filter the Predefined Patterns
        https://github.com/elasticsearch/logstash/tree/v1.4.0/patterns
        As BASE10NUM the Predefined Pattern is - [+ -] ((? <([0-9 +.]!)?>:????. (: [0-9] + (: \ [0-9] + )) | (:??. \ [0-9] +)))

    (Important) http://grokdebug.herokuapp.com/
        It can be used for learning and testing a website Grok match





3.Elasticsearch Introduction

    (Must see) Elasticsearch: Search made easy for (web) developers
        http://spinscale.github.io/elasticsearch/2012-03-jugm.html#/

    (Must see) Getting Down and dirty with Elasticsearch
        http://www.slideshare.net/clintongormley/down-and-dirty-with-elasticsearch
        200pages + the slide, the Rest API to Elasticsearch introduce more

    (Must see) Elasticsearch: Pluggable architecture under the hood
        http://spinscale.github.io/elasticsearch-intro-plugins/#/

    (Must see) Exploring Elasticsearch
        http://exploringelasticsearch.com/
        System introduced Elasticsearch, of course, the book "Elasticsearch Server" is more comprehensive than it, more details

    (Must see) Terms of endearment - the ElasticSearch Query DSL explained
        http://www.slideshare.net/clintongormley/terms-of-endearment-the-elasticsearch-query-dsl-explained

    Apache Lucene - Query Parser Syntax
        (1) http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax
        (2) http://lucene.apache.org/core/4_6_0/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#Overview
        Lucene's Query syntax

    Learning Elasticsearch
        http://slides.com/garyelephant/learning-elasticsearch
        I recommend making a slide, with reference to a lot of information set 100 of the director, targeted to do, all aspects of presentation Elasticsearch, content constantly updated.

    (Important) Elasticsearch References: Glossary of terms
        http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/glossary.html#glossary
        Some of the core concepts Elasticsearch

    (Important) What is an ElasticSearch Index?
        http://euphonious-intuition.com/2013/02/what-is-an-elasticsearch-index/
        Details Index
    (Important) An introduction to mapping in elasticsearch
        http://euphonious-intuition.com/2012/07/an-introduction-to-mapping-in-elasticsearch/
    
    Elasticsearch Facets
        http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/search-facets.html
    
    Elasticsearch Aggregations Overview
        http://chrissimpson.co.uk/elasticsearch-aggregations-overview.html

    Elastic Search Training # 1 (brief tutorial) -ESCC # 1
        http://www.slideshare.net/medcl/elastic-search-training1-brief-tutorial

    Lucene Scoring and elasticsearch's _all Field
        http://jontai.me/blog/2012/10/lucene-scoring-and-elasticsearch-_all-field/
        Score calculation Elasticsearch data
    
    Advanced Scoring in elasticsearch
        http://jontai.me/blog/2013/01/advanced-scoring-in-elasticsearch/
        Score calculation Elasticsearch data





4.Elasticsearch optimization

    elasticsearch configuration
        http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/setup-configuration.html#setup-configuration-memory

    elasticsearch configuration and performance tuning.html
        http://weiweiwang.github.io/elasticsearch-configuration-and-performance-tuning.html

    ElasticSearch Training # 2 (advanced concepts) -ESCC # 1
        http://www.slideshare.net/medcl/elastic-search-training2-advanced-concepts
        Elasticsearch performance optimization, query optimization techniques

    Elasticsearch Java Virtual Machine settings explained
        http://jprante.github.io/2012/11/28/Elasticsearch-Java-Virtual-Machine-settings-explained.html

    ElasticSearch and Logstash Tuning
        http://jablonskis.org/2013/elasticsearch-and-logstash-tuning/
    
    Elasticsearch Plugin: Marvel
        http://www.elasticsearch.org/overview/marvel/
        Elasticsearch.org released Elasticsearch cluster monitoring tool

    Elasticsearch Plugin: Head
        https://github.com/mobz/elasticsearch-head

    Elasticsearch Plugin: Bigdesk - Live charts and statistics for elasticsearch cluster.
        http://bigdesk.org/

    Elasticsearch Plugin: Paramedic
        https://github.com/karmi/elasticsearch-paramedic

    Scaling Massive Elasticsearch Clusters
        http://www.slideshare.net/sematext/scaling-massive-elasticsearch-clusters
        Elasticsearch Cluster





5.Kibana Introduction

    (Must see) 10 Minute Walk Through Kibana
        http://www.elasticsearch.org/guide/en/kibana/current/using-kibana-for-the-first-time.html
        Elasticsearch.org Official Guide, recently released.
    

    Kibana Overview
        http://www.elasticsearch.org/overview/kibana/
    

    What's Cooking in Kibana
        http://www.elasticsearch.org/blog/whats-cooking-kibana/
    

    Kibana 3: Milestone 4
        http://www.elasticsearch.org/blog/kibana-3-milestone-4/
        New features Kibana3.m4 of

    Kibana entry -Yusuke Mito
        https://speakerdeck.com/y310/kibanaru-men?slide=23
        page23 ~ page37 there are shots of various panel



6. The most important fundamental Resources

  (1) Elasticsearch team blog, updated weekly
      http://www.elasticsearch.org/blog

  (2) Elasticsearch latest version of API References
      http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/index.html
      
  (3) logstash 1.4.0 of documents
      http://logstash.net/docs/1.4.0/

  (4) Kibana latest version of documents
      http://www.elasticsearch.org/guide/en/kibana/current/index.html


7. recommend two books

    "Elasticsearch Server"
    "Mastering Elasticsearch"




