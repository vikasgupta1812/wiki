Configuration for getting logs - https://github.com/sbagmeijer/ulyaoth/blob/master/guides/logstash/windows/logstash.conf

Documentation with mention of log shipper
Provides more details about configuration file
It can reference to live file
- http://logstash.net/docs/1.0.17/getting-started-centralized

Tutorial about configuring nxlog as log shipper 
benefits are that it can read windows event logs as well 
http://www.ragingcomputer.com/2014/02/sending-windows-event-logs-to-logstash-elasticsearch-kibana-with-nxlog

http://www.ragingcomputer.com/2014/02/securing-elasticsearch-kibana-with-nginx

Recipies for configuring the windows service
https://github.com/logstash/cookbook/tree/gh-pages/recipes/windows-service

List of log shippers 
https://github.com/logstash/cookbook/blob/gh-pages/recipes/log-shippers/index.md

Discussion about log shipper 
http://stackoverflow.com/questions/25685650/why-do-people-ship-logs-to-logstash-with-nxlog-and-not-logstash-itself

Getting Kibana Up and Running
http://www.elastic.co/guide/en/kibana/current/setup.html
The first time you access Kibana, you are prompted to define an index pattern that matches the name of one or more of your indices.

You can add index patterns at any time from the Settings tab

By default, Kibana connects to the Elasticsearch instance running on localhost. To connect to a different Elasticsearch instance, modify the Elasticsearch URL in the kibana.yml configuration file and restart Kibana.


Using Kibana in a Production Environment
http://www.elastic.co/guide/en/kibana/current/production.html

Kibana 4 Tutorial – Part 1: Introduction
https://www.timroes.de/2015/02/07/kibana-4-tutorial-part-1-introduction/


Search Elastic Search - http://localhost:9200/_search
http://localhost:9200/_mapping?pretty
http://localhost:9200/logstash-2015.05.14/_mapping?pretty
