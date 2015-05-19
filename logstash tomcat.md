Logstash - Oh no, more logs, start with logstash 
====

http://www.gridshore.nl/2013/11/10/oh-no-more-logs-start-with-logstash/

Provides information abuot how to access tomcat access logs. If you want to obtain access logs in tomcat you need to add a valve to the configured host in server.xml.
 
```
<Valve className="org.apache.catalina.valves.AccessLogValveDC"
       directory="/Users/jcoenradie/temp/logs/"
       prefix="localhost_access_log"
       suffix=".txt"
       renameOnRotate="true"
       pattern="%h %t %S &quot;%r&quot; %s %b" />
```

An example output from the logs than is, the table shows what the pattern we have means

```
h	remote host
t	timestamp
S	session id
r	first line of request
s	http status code of response
b	bytes send
```

If you want mote information about the logging options check the [tomcat configuration](http://tomcat.apache.org/tomcat-7.0-doc/config/valve.html#Access_Log_Valve). 

First step is get the contents of this file into logstash. Therefore we have to make a change to add an input coming from a file.


```
input {
  stdin { }
  file {
    type => "tomcat-access"
    path => ["/Users/jcoenradie/temp/dpclogs/localhost_access_log.txt"]
  }
}
output {
  stdout { }
 
  elasticsearch {
    cluster => "logstash"
  }
}
```

Logstash filtering

You can use filters to enhance the received events. The following configuration shows how to extract client, timestamp, session id, method, uri path, uri param, protocol, status code and bytes.

```
input {
  stdin { }
  file {
    type => "tomcat-access"
    path => ["/Users/jcoenradie/temp/dpclogs/localhost_access_log.txt"]
  }
}
filter {
  if [type] == "tomcat-access" {
    grok {
      match => ["message","%{IP:client} \[%{TIMESTAMP_ISO8601:timestamp}\] (%{WORD:session_id}|-) \"%{WORD:method} %{URIPATH:uri_path}(?:%{URIPARAM:uri_param})? %{DATA:protocol}\" %{NUMBER:code} (%{NUMBER:bytes}|-)"]
    }
  }
}
output {
  stdout { }
 
  elasticsearch {
    cluster => "logstash"
  }
}
```