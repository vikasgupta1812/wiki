
### your-logs-are-your-data-logstash-elasticsearch
http://www.javacodegeeks.com/2013/02/your-logs-are-your-data-logstash-elasticsearch.html

### Run elastic search 

```
elasticsearch.bat -Des.index.store.type=memory -Des.network.host=localhost
```

configuration file. 
```
input {
    file {
        add_field => [ 'host', 'my-dev-host' ]
        path => 'c:\tmp\application.log'
        type => 'app'
        format => 'plain'
    }
}

output {
    elasticsearch_http {
        host => 'localhost'
        port => 9200 
        type => 'app'
        flush_size => 10
    }
}

filter {
    multiline {
        type => 'app'
        pattern => '^[^\[]'
        what => 'previous'  
    }
}
```

input is c:\tmp\application.log, 
which is a plain text file (format => ‘plain’). 
The type => ‘app’ serves as simple marker so the different types of inputs could be routed to outputs through filters with the same type. 
The add_field => [ ‘host’, ‘my-dev-host’ ] allows to inject additional arbitrary data into the incoming stream, f.e. hostname.


The multiline filter will glue the stack trace to the log statement it belongs to so it will be stored as a single (large) multiline



```
java -cp logstash-1.1.9-monolithic logstash.runner agent -f logstash.conf
```


Install plugin from downloaded file. 

```
C:\Logstash\elasticsearch\bin>plugin.bat -u "file:///C:/Users/q02874/AppData/Local/Temp/elasticsearch-kopf-master.zip" -i lmenezes/elasticsearch-kopf
-> Installing lmenezes/elasticsearch-kopf...
Trying file:/C:/Users/q02874/AppData/Local/Temp/elasticsearch-kopf-master.zip...
Downloading ....................DONE
Installed lmenezes/elasticsearch-kopf into C:\Logstash\elasticsearch\plugins\kopf
Identified as a _site plugin, moving to _site structure ...
```




If you want to delete old indices after a certain time period, you can use the Elasticsearch Curator tool.
http://www.elastic.co/guide/en/elasticsearch/client/curator/current/index.html


-----------

Tomcat Manager: 
xml 
- https://www.elastic.co/guide/en/logstash/current/plugins-filters-xml.html
- best practice for XML data / elasticsearch https://groups.google.com/forum/#!topic/logstash-users/e6pQLQ_hBZ0

-----------
- 9 uses for cURL worth knowing - http://httpkit.com/resources/HTTP-from-the-Command-Line/

-----------

Download Json data from cpp site. 

Stack overflow find and replace (with regex)
```
\}, \{ 
\}\n\{ 
```

Remove leading and trailing junk from json output
```
sed s/"{ \"fProcesses\" : \["//g prodlog.22-05-2015.json
sed s/"}\]"//g prodlog.22-05-2015.json
```

Attempt to have the new line character entered by sed -- unsuccessful. 
```
sed s/"}, {"/"} \
{"/g prodlog.22-05-2015.json | wc -l
```
It seems that new logstash supports delimiter clause, where ", " can be defined as delimiter. 
----


Email output - https://www.elastic.co/guide/en/logstash/current/plugins-outputs-email.html



---- 
LumberJack --> logstash forwarder

https://logstash.jira.com/browse/LOGSTASH-1856

"go build" the project, it creates a "logstash-forwarder-master.exe" executable in the base directory.

And don't forget that the path names in Windows need to be of the format : `c:\\apps\\mylogs`



----
How to Logstash-forwarder as Windows Service http://stackoverflow.com/questions/29610991/logstash-forwarder-as-windows-service

1- Get nssm soft
2- Decompress the nssm zip in the bin folder of logstash
3- Excecute from command line nssm install logstash
4- Add the path to your bat on the launched config screen
5- Add your startup directory too.

----
public-notes logstash-windows.md

----
I use NXLog to ship Windows event logs, IIS logs and etc to the ELK stack without any issues. Ping me if you have any questions or issues.

http://everythingshouldbevirtual.com/highly-available-elk-elasticsearch-logstash-kibana-setup﻿

----

Logstash File configuration options:

`delimiter`

Value type is string
Default value is "\n"
set the new line delimiter, defaults to "\n"


----


Resolve host name from IP address `nslookup`
http://serverfault.com/questions/74042/resolve-host-name-from-ip-address