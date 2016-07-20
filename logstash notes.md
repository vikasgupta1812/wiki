
## TODO

- Install on AWS 2012 IIS 
- Install on Win7 Tomcat 8 
- Configure Existing logs 
- Build Dashboards in Kibana
- Configure Tomcat log4j for access logs
- Configure Server for Windows logs
- Create Presentations

## Logstash configuration

- For getting IIS logs - https://github.com/sbagmeijer/ulyaoth/blob/master/guides/logstash/windows/logstash.conf
- https://github.com/elastic/logstash/issues/1382
- http://www.logstashbook.com/code/6/shipper.conf


## Multiline Codec

- http://stackoverflow.com/questions/24457004/logstash-1-4-1-multiline-codec-not-working
- [Logstash Multiline Filter for Java Stacktrace (tested on field)](http://webcache.googleusercontent.com/search?q=cache:4TVOi2erYS4J:https://gist.github.com/3182192+&cd=7&hl=en&ct=clnk&gl=us) - Code with example
- [Use rsyslog as a local agent to ship logs to logstash.](https://github.com/logstash/cookbook/blob/gh-pages/recipes/rsyslog-agent/index.html)

## Installers 

- Logstash 1.5.0 - http://download.elastic.co/logstash/logstash/logstash-1.5.0.zip

## Log Shipper

- Documentation  http://logstash.net/docs/1.0.17/getting-started-centralized
Provides more details about configuration file
can reference to live file
- Tutorial about configuring `nxlog` as log shipper http://www.ragingcomputer.com/2014/02/sending-windows-event-logs-to-logstash-elasticsearch-kibana-with-nxlog
It can read windows event logs
Other tuts from same blog - http://www.ragingcomputer.com/2014/02/securing-elasticsearch-kibana-with-nginx
- How to Start logstash on boot up. - Logstash Cookbook Recipie - https://github.com/logstash/cookbook/tree/gh-pages/recipes/windows-service
- List of log shippers https://github.com/logstash/cookbook/blob/gh-pages/recipes/log-shippers/index.md
- stackoverflow - Discussion about log shipper - http://stackoverflow.com/questions/25685650/why-do-people-ship-logs-to-logstash-with-nxlog-and-not-logstash-itself

## Kibana setup 

- Getting Kibana Up and Running - http://www.elastic.co/guide/en/kibana/current/setup.html
The first time you access Kibana, you are prompted to define an `index pattern` that matches the name of one or more of your indices. You can add index patterns at any time from the Settings tab
By default, Kibana connects to the Elasticsearch instance running on localhost. To connect to a different Elasticsearch instance, modify the Elasticsearch URL in the kibana.yml configuration file and restart Kibana.
- Using Kibana in a Production Environment - http://www.elastic.co/guide/en/kibana/current/production.html
- ![Anna Roes](https://2.gravatar.com/avatar/eaac33b12e49df64af0fc522aeb6e460?s=96&amp;d=mm&amp;r=g)Kibana 4 Tutorial – Part 1: Introduction - https://www.timroes.de/2015/02/07/kibana-4-tutorial-part-1-introduction/ 

## Elasticsearch urls 

- Search Elastic Search - http://localhost:9200/_search
- http://localhost:9200/_mapping?pretty
- http://localhost:9200/logstash-2015.05.14/_mapping?pretty
- http://localhost:9200/_cat/indices?v - Index status 

```
health status index               pri rep docs.count docs.deleted store.size pri.store.size 
yellow open   logstash-2015.05.14   5   1         22            0    154.6kb        154.6kb 
yellow open   .kibana               1   1          2            0      6.7kb          6.7kb
```

## Logstash-Grok

- [Little Logstash Lessons - Part I: Using grok and mutate to type your data](https://www.elastic.co/blog/little-logstash-lessons-part-using-grok-mutate-type-data)
- Grok Patterns = https://github.com/elastic/logstash/blob/v1.4.2/patterns/grok-patterns
- http://logstash.net/docs/1.0.17/filters/grok

##Find patterns for Reliance log

Matching Patterns found so far. 

```
%{YEAR}[/-]%{MONTHNUM}[/-]%{MONTHDAY}[T ]%{HOUR}:?%{MINUTE}:?%{SECOND}
%{TIMESTAMP_ISO8601} (ERROR|FATAL|INFO)
%{TIMESTAMP_ISO8601} %{SYSLOG5424SD}
%{TIMESTAMP_ISO8601} %{SYSLOG5424SD} %{LOGLEVEL} %{JAVACLASS}
```

## File inputs 
http://logstash.net/docs/1.3.2/inputs/file

By default, each event is assumed to be one line. If you want to join lines, you'll want to use the multiline filter.

Files are followed in a manner similar to "tail -0F". File rotation is detected and handled by this input.

## Logstash Multiline Tomcat and Apache Log Parsing

- http://blog.lanyonm.org/articles/2014/01/12/logstash-multiline-tomcat-log-parsing.html

Sample log

```
Jan 9, 2014 7:13:13 AM org.apache.tomcat.util.http.Parameters processParameters
INFO: Character decoding failed. Parameter [dcp] with value [ppn.%epid!.] has been ignored. Note that the name and value quoted here may be corrupted due to the failed decoding. Use debug level logging to see the original, non-corrupted values.
 Note: further occurrences of Parameter errors will be logged at DEBUG level.
2014-01-09 17:32:25,527 -0800 | ERROR | com.example.controller.ApiController - Request exception
javax.xml.ws.WebServiceException: Failed to access the WSDL at: https://api.example.com/DataServices/Data?WSDL. It failed with:
    Connection reset.
    at com.example.webservices.Data.<init>(Data.java:50)
    at com.example.service.soap.DataService.submitRequest(DataService.groovy:28)
    at com.example.service.request.RequestService.addRequest(RequestService.groovy:26)
    at com.example.controller.ApiController.request(ApiController.groovy:692)
    at grails.plugin.cache.web.filter.PageFragmentCachingFilter.doFilter(PageFragmentCachingFilter.java:200)
    at grails.plugin.cache.web.filter.AbstractFilter.doFilter(AbstractFilter.java:63)
    at org.apache.jk.server.JkCoyoteHandler.invoke(JkCoyoteHandler.java:190)
    at org.apache.jk.common.HandlerRequest.invoke(HandlerRequest.java:311)
    at org.apache.jk.common.ChannelSocket.invoke(ChannelSocket.java:776)
    at org.apache.jk.common.ChannelSocket.processConnection(ChannelSocket.java:705)
    at org.apache.jk.common.ChannelSocket$SocketConnection.runIt(ChannelSocket.java:898)
Caused by: java.net.SocketException: Connection reset
    ... 17 more
```
Parse the message and convert time to timestring.

```
filter {
  if [type] == "apache" {
    grok {
      patterns_dir => "/Users/lanyonm/logstash/patterns"
      match => { "message" => "%{COMBINEDAPACHELOG}" }
    }
    date {
      match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
    }
  }
```

The `multiline filter` is the key for Logstash to understand log events that span multiple lines. In my case, each Tomcat log entry began with a timestamp, making the timestamp the best way to detect the beginning of an event.

`multiline filter` - http://logstash.net/docs/1.3.2/filters/multiline

- my grok-patterns gist: https://gist.github.com/LanyonM/8390458#file-grok-patterns-L101

```
CATALINA_DATESTAMP %{MONTH} %{MONTHDAY}, 20%{YEAR} %{HOUR}:?%{MINUTE}(?::?%{SECOND}) (?:AM|PM)
TOMCAT_DATESTAMP 20%{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{HOUR}:?%{MINUTE}(?::?%{SECOND}) %{ISO8601_TIMEZONE}
```

- Using these two patterns, we are able to construct the `multiline pattern` to match both conversion patterns. The `negate` and `previous` mean that each line will log-line rolls into the previous lines unless the pattern is matched.

```
  if [type] == "tomcat" {
    multiline {
      patterns_dir => "/Users/lanyonm/logstash/patterns"
      pattern => "(^%{TOMCAT_DATESTAMP})|(^%{CATALINA_DATESTAMP})"
      negate => true
      what => "previous"
    }
```

The next thing to do is `parse` each event into its constituent parts. In the Tomcat log example above, the timestamp is followed by a `logging level`, `classname` and `log message`. Grok already provides some of of these patterns, so we just had to glue them together. Again, because there are two different syntaxes for a log statement, we have two patterns:

```
CATALINALOG %{CATALINA_DATESTAMP:timestamp} %{JAVACLASS:class} %{JAVALOGMESSAGE:logmessage}
TOMCATLOG %{TOMCAT_DATESTAMP:timestamp} \| %{LOGLEVEL:level} \| %{JAVACLASS:class} - %{JAVALOGMESSAGE:logmessage}
```

We see some of the filter nuance below. These two `patterns` can be checked against an `event` by specifying the `match` with a `hash` of comma-separated `keys` and `values`. The grok filter will attempt to match each pattern before failing to parse. The filter’s [match documentation](http://logstash.net/docs/1.3.2/filters/grok#match) isn’t quite perfected on this point yet. Have a look at the grok `filter` below:

```
    if "_grokparsefailure" in [tags] {
      drop { }
    }
    grok {
      patterns_dir => "/Users/lanyonm/logstash/patterns"
      match => [ "message", "%{TOMCATLOG}", "message", "%{CATALINALOG}" ]
    }
    date {
      match => [ "timestamp", "yyyy-MM-dd HH:mm:ss,SSS Z", "MMM dd, yyyy HH:mm:ss a" ]
    }
  }
}
```

Inevitably, there will be `mess` in your logs that doesn’t conform to your grok parser. You can choose to drop events that fail to parse by using the drop filter inside a conditional as shown on the second line above. Where do `[tags]` come from you might ask?

`Tags` can be applied to events at several points in the processing pipeline. For example, when the `multiline` filter successfully parses an event, it tags the event with `multiline`.

The `date` filter can accept a comma separated list of timestamp patterns to match. This allows either the `CATALINA_DATESTAMP` pattern or the `TOMCAT_DATESTAMP` pattern to match the date filter and be ingested by Logstash correctly.

### Output
The output is simply an embedded Elasticsearch config as well as debugging to stdout. If you’d like to see the full config, have a look at the [gist](https://gist.github.com/LanyonM/8390458#file-logstash-java-conf).

Full Config 

```json
input {
	tcp {
		type => "apache"
		port => 3333
		add_field => { "server" => "prod1" }
	}
	tcp {
		type => "apache"
		port => 3334
		add_field => { "server" => "prod2" }
	}
	tcp {
		type => "tomcat"
		port => 3335
		add_field => { "server" => "prod1" }
	}
	tcp {
		type => "tomcat"
		port => 3336
		add_field => { "server" => "prod2" }
	}
}

filter {
	if [type] == "apache" {
		grok {
			patterns_dir => "/Users/lanyonm/logstash/patterns"
			match => { "message" => "%{COMBINEDAPACHELOG}" }
		}
		date {
			match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
		}
	}
	if [type] == "tomcat" {
		multiline {
			patterns_dir => "/Users/lanyonm/logstash/patterns"
			pattern => "(^%{TOMCAT_DATESTAMP})|(^%{CATALINA_DATESTAMP})"
			negate => true
			what => "previous"
		}
		if "_grokparsefailure" in [tags] {
			drop { }
		}
		grok {
			patterns_dir => "/Users/lanyonm/logstash/patterns"
			match => [ "message", "%{TOMCATLOG}", "message", "%{CATALINALOG}" ]
		}
		date {
			match => [ "timestamp", "yyyy-MM-dd HH:mm:ss,SSS Z", "MMM dd, yyyy HH:mm:ss a" ]
		}
	}
}

output {
	if [type] == "tomcat" and "_grokparsefailure" in [tags] {
	   # if we didn't drop the messages above, we could send them to a special failure log here
	}
	stdout {
		debug => true
	}
	elasticsearch {
		embedded => true
	}
}
```

### Grok Patterns

There’s no magic to grok patterns (unless the built-ins work for you). There are however a couple resources that can make your parsing go faster. 

First is the [Grok Debugger](http://grokdebug.herokuapp.com/). You can paste messages into the Discover tab and the Debugger will find the best matches against the built in patterns. 

Another regex assistant I use is [RegExr](http://regexr.com/). I have the native app, but the web page is nice too.

The **full list** of patterns shipped with Logstash can be found [on GitHub](https://github.com/logstash/logstash/tree/master/patterns), and the ones I used can be found in this Gist. If you’re not into clicking links, here are the important ones:

https://github.com/elastic/logstash/blob/v1.4.2/patterns/grok-patterns
https://github.com/elastic/logstash/commit/12ee26f43b18a1d1b651ffd0a71854ba2302ebd5#diff-9dea51b41ad380aa83f2a25a7cecc5eb
https://github.com/elastic/logstash/blob/1.4/patterns/grok-patterns
https://github.com/elastic/logstash/blob/1.3.x/patterns/grok-patterns


```
JAVACLASS (?:[a-zA-Z0-9-]+\.)+[A-Za-z0-9$]+
JAVALOGMESSAGE (.*)

# MMM dd, yyyy HH:mm:ss eg: Jan 9, 2014 7:13:13 AM
CATALINA_DATESTAMP %{MONTH} %{MONTHDAY}, 20%{YEAR} %{HOUR}:?%{MINUTE}(?::?%{SECOND}) (?:AM|PM)

# yyyy-MM-dd HH:mm:ss,SSS ZZZ eg: 2014-01-09 17:32:25,527 -0800
TOMCAT_DATESTAMP 20%{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{HOUR}:?%{MINUTE}(?::?%{SECOND}) %{ISO8601_TIMEZONE}
CATALINALOG %{CATALINA_DATESTAMP:timestamp} %{JAVACLASS:class} %{JAVALOGMESSAGE:logmessage}

# 2014-01-09 20:03:28,269 -0800 | ERROR | com.example.service.ExampleService - something compeletely unexpected happened...
TOMCATLOG %{TOMCAT_DATESTAMP:timestamp} \| %{LOGLEVEL:level} \| %{JAVACLASS:cl
```

[Viewing tomcat logs with Logstash in a Windows 7 machine](http://dotnetanalysis.blogspot.com/2014/11/viewing-tomcat-logs-with-logstash-in.html)

#### Input: 

This tells logstash where the data is coming from. For example File, eventlog, twitter, tcp and so on. All the supported inputs can be found here. http://logstash.net/docs/1.4.2/

- http://logstash.net/docs/1.4.2/inputs/syslog - This input is a good choice if you already use syslog today. It is also a good choice if you want to receive logs from appliances and network devices where you cannot run your own log collector.

- http://logstash.net/docs/1.4.2/inputs/log4j - Read events over a TCP socket from a Log4j SocketAppender.
Can either accept connections from clients or connect to a server, depending on mode. Depending on which mode is configured, you need a matching SocketAppender or a SocketHubAppender on the remote side.

- http://logstash.net/docs/1.4.2/inputs/file - Files are followed in a manner similar to “tail -0F”. File rotation is detected and handled by this input.

- http://logstash.net/docs/1.4.2/inputs/wmi - Collect data from WMI query
This is useful for collecting performance metrics and other data which is accessible via WMI on a Windows host

#### Filter: 

This tells logstash what you want to do to the data before you output it into your log store (in our case elasticsearch). This accepts regular expressions. Most people use precreated regular expressions called grok, instead of writing their own regular expressions to parse log data. The complete list of filters you can use can be seen here.

#### Output: 

This tells logstash where to output this filtered data to. We are going to output it into elasticsearch. You can have multiple outputs if you want.

This is how my logstash.conf looks like 

```json
input {
    file {        
    path => ["C:/Program Files/apache-tomcat-7.0.55/logs/*.log"]
    }
}

filter {
 
}
output{

   elasticsearch {
     cluster=>"VivekLocalMachine"
      port => "9200"
      protocol => "http"
    }
}
```

The cluster name should match what you set in C:\Program Files\elasticsearch-1.3.4\config\elasticsearch.yml


## Logstash config for IIS logs
- http://dotnetanalysis.blogspot.com/2014/11/logstash-config-for-iis-logs.html

```json
input {
    file {     
    path => ["C:/inetpub/logs/LogFiles/W3SVC1/*.log"]
     type => ["iislog"]
    } 
 file {     
      path => ["C:/Program Files/apache-tomcat-7.0.55/logs/*.txt"]
      type => ["tomcatTxtLog"]
    } 
}


filter {
  if [type] == "iislog" {
         #ignore log comments
         if [message] =~ "^#"
         {
           drop {}
         }
   grok {
     match => ["message", "%{TIMESTAMP_ISO8601:log_timestamp} %{WORD:iisSite} %{IPORHOST:site} %{NOTSPACE:Sip} %{NOTSPACE:verb} %{URIPATH:request} %{NOTSPACE:QueryString} %{NUMBER:port} %{NOTSPACE:Hyphen1} %{NOTSPACE:Cip} %{NOTSPACE:httpversion} %{NOTSPACE:UserAgent} %{NOTSPACE:Hyphen2} %{NOTSPACE:Hyphen3} %{NOTSPACE:referer} %{NUMBER:response} %{NUMBER:subresponse} %{NUMBER:scstatus} %{NUMBER:bytes:int} %{NUMBER:timetaken:int}"]
        }
   date
   {
      match => [ "log_timestamp", "YYYY-MM-dd HH:mm:ss" ]
      timezone => "Etc/GMT"
   }
   mutate {
     remove_field => [ "Hyphen1","Hyphen2","Hyphen3","Sip","Cip","log_timestamp"]
     }
   }
  else if [type] == "tomcatTxtLog" {
  #ignore log comments
  if [message] =~ "^#"
   {
            drop {}
         }
         grok {
           match => ["message", "%{COMMONAPACHELOG}"]
           }
     date
     {     
            match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
            timezone => "Etc/GMT"
           }
      mutate {
            remove_field => [ "timestamp"]
         }
 
     }
}

output{
   elasticsearch {
     cluster=>"VivekLocalMachine"
      port => "9200"
      protocol => "http"
    }
 }
```
## Accessing Tomcat manager 

- To be able to access your tomcat manager app at https://localhost:8443/ add this to apache-tomcat-7.0.55\conf\tomcat-users.xml

```
<tomcat-users>
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <role rolename="manager-jmx"/>
  <role rolename="manager-status"/>
  <role rolename="admin-gui"/>
  <role rolename="admin-script"/>
  <user username="tomcat" password="tomcat" roles="manager-gui,manager-script,manager-jmx,manager-status,admin-gui,admin-script"/>
  </tomcat-users>
```

Now you can access everything in the manager gui using

```
username: tomcat
password: tomcat
```

----
## Oh no, more logs, start with logstash 


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
h remote host
t timestamp
S session id
r first line of request
s http status code of response
b bytes send
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

```json
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

Another post showing how to access\configure tomcat logs 

https://spredzy.wordpress.com/2013/03/02/monitor-your-cluster-of-tomcat-applications-with-logstash-and-kibana/


Logs Access

In order to enable logs access in tomcat edit your /usr/share/tomcat7/conf/server.xml and add the AccesLog valve

```
<Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
    prefix="localhost_access_log." suffix=".txt" renameOnRotate="true"
    pattern="%h %l %u %t &quot;%r&quot; %s %b" />
```

Application logs 

Here a library that will output log4j message directly to the logstash json_event format will be used

Configure log4j.xml


Edit the /usr/share/tomcat7/wepapps/myapp1/WEB-INF/classes/log4j.xml
```
<appender name="MYLOGFILE" class="org.apache.log4j.DailyRollingFileAppender">
    <param name="File" value="/path/to/my/log.log"/>
    <param name="Append" value="false"/>
    <param name="DatePattern" value="'.'yyyy-MM-dd"/>
    <layout class="net.logstash.log4j.JSONEventLayout"/>
</appender>
```

and more.. 


----

## Logstash Book Configurations
Shipper configuration - http://logstashbook.com/code/6/shipper.conf


----
## How to Delete ElasticSearch Index
- https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-delete-index.html

```
$curl -XDELETE localhost:9200/twitter/
{"error":"IndexMissingException[[twitter] missing]","status":404}
```
The above example deletes an index called twitter. Specifying an index, alias or wildcard expression is required.

```
$curl -XDELETE localhost:9200/_all/
{"acknowledged":true}
```

The delete index API can also be applied to more than one index, or on all indices (be careful!) by using _all or * as index. http://stackoverflow.com/a/22932471


####Delete Mapping [Source](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-delete-mapping.html)

Allow to delete a mapping (type) along with its data. The REST endpoints are

```
[DELETE] /{index}/{type}
[DELETE] /{index}/{type}/_mapping
[DELETE] /{index}/_mapping/{type}
```
where

index
```
* | _all | glob pattern | name1, name2, …
```
type
```
* | _all | glob pattern | name1, name2, …
```
Note, most times, it make more sense to reindex the data into a fresh index compared to delete large chunks of it.






----
## HOWTO: Install Tomcat 

http://www.ntu.edu.sg/home/ehchua/programming/howto/Tomcat_HowTo.html

----

## HOWTO: Configure Access Logging in Tomcat

http://www.techstacks.com/howto/configure-access-logging-in-tomcat.html

Tomcat access logging is enabled by modifying the server.xml file and uncommenting the Access Log Valve.   In a default tomcat implementation, the access log valve section is located within the Host element.   Uncommenting the entry will enable an access log that contains fields equivalent to a "common" log file format from Apache. 

The defaults for the valve will result in a file named "localhost_access_log" followed by the date, followed by a ".txt" file extension.   IP addresses will be logged, not hostnames and log file will be written into the ${tomcat.home}/logs directory.   The fields present in the log file using a common format are:

  - Client host name (recorded as an IP if the default resolveHosts is not changed to "true").
  - Remote logical username (which always prints a "-").
  - Remote authenticated user ID (if one exists)
  - Date and Time of the request
  - HTTP Method and URI requested
  - HTTP Response Status Code
  - Size, in bytes, of the response (excluding http response headers)

### Customizing the Access Log

The common log format is ok but changing the pattern to combined adds the User-Agent (browser or robot type) and the referring web site and URI.   Tomcat also provides additional options to log things like the `request protocol`, the `local port` that received the request, `user session ID's`, `incoming` or `outgoing` `request headers`, etc.   A full list is documented at the [Tomcat Configuration Reference Valve Component page](http://tomcat.apache.org/tomcat-6.0-doc/config/valve.html#Access_Log_Valve).

If you are running a version of tomcat greater than version 6.0.21 or tomcat 7, you can take advantage of the new `Remote IP Valve`. For access logging, the nice thing about this valve is that it will swap the client IP with an IP address passed with the X-Forwarded-For header—automatically—if an IP address is passed in the X-Forwarded-For header.  Loading it is pretty easy. Just add the org.apache.catalina.valves.RemoteIpValve to your server.xml before your AccessLogValve declaration. For example:

```
    <Valve className="org.apache.catalina.valves.RemoteIpValve" />
    <!-- Access log processes all example.
      Documentation at: /docs/config/valve.html -->
         
    <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs" 
      prefix="localhost_access_log." suffix=".txt"
      pattern="combined" resolveHosts="false"/>
```
----
[Monitor your cluster of Tomcat applications with Logstash and Kibana](https://spredzy.wordpress.com/2013/03/02/monitor-your-cluster-of-tomcat-applications-with-logstash-and-kibana/)

Logs Access

```
<Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
    prefix="localhost_access_log." suffix=".txt" renameOnRotate="true"
    pattern="%h %l %u %t &quot;%r&quot; %s %b" />
```

Application Logs

Here a library that will output log4j message directly to the logstash json_event format will be used

Special thanks to [@lusis](https://twitter.com/lusis) for the hard work – so no groking will be required

Jar : [jsonevent-layout](http://search.maven.org/remotecontent?filepath=net/logstash/log4j/jsonevent-layout/1.3/jsonevent-layout-1.3.jar)
Dependency : [json-smart](http://json-smart.googlecode.com/files/json-smart-1.1.1.jar)
Configure log4j.xml

Edit the /usr/share/tomcat7/wepapps/myapp1/WEB-INF/classes/log4j.xml

```
<appender name="MYLOGFILE" class="org.apache.log4j.DailyRollingFileAppender">
    <param name="File" value="/path/to/my/log.log"/>
    <param name="Append" value="false"/>
    <param name="DatePattern" value="'.'yyyy-MM-dd"/>
    <layout class="net.logstash.log4j.JSONEventLayout"/>
</appender>
```




-----
## Other links


[Logstash Tutorial: Using Logstash to Streamline Email Notifications](http://www.toptal.com/java/using-logstash-to-streamline-email-notifications)

[How to Setup Logstash on Linux with ElasticSearch, Redis, Nginx](http://www.thegeekstuff.com/2014/12/logstash-setup/)

[Using logstash, elasticsearch and kibana to monitor your video card – a tutorial](http://blog.trifork.com/2014/01/28/using-logstash-elasticsearch-and-kibana-to-monitor-your-video-card-a-tutorial/)

[5-minute Logstash: Parsing and Sending a Log File](http://blog.sematext.com/2013/12/19/getting-started-with-logstash/)

[Logstash - Windows install notes](https://github.com/verbosemode/public-notes/blob/master/logstash-windows.html)

[How to force Logstash to reparse a file?](http://stackoverflow.com/questions/19546900/how-to-force-logstash-to-reparse-a-file)

[How To Use Logstash and Kibana To Centralize Logs On Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-use-logstash-and-kibana-to-centralize-and-visualize-logs-on-ubuntu-14-04)

[Logstash Multiline Tomcat and Apache Log Parsing](http://blog.lanyonm.org/articles/2014/01/12/logstash-multiline-tomcat-log-parsing.html)

http://stackoverflow.com/questions/26828099/kibana-returns-connection-failed

https://groups.google.com/forum/#!topic/logstash-users/7-cxJLlhDD0

http://www.reddit.com/r/sysadmin/comments/2m8qzf/sysadmins_what_do_you_do_with_your_logs/

Nice presentation: http://www.soit.sk/media/a542/file/item/sk/0000/elk_stack_alexander_szalonnas.w5s3.pdf

http://www.cybertechquestions.com/how-to-setup-logstash-on-azure-with-windows-configuration_1114421.html

[Logstash date parsing as timestamp using the date filter](http://stackoverflow.com/questions/25156517/logstash-date-parsing-as-timestamp-using-the-date-filter) 


------------

```
%a - Remote IP address
%A - Local IP address
%b - Bytes sent, excluding HTTP headers, or '-' if zero
%B - Bytes sent, excluding HTTP headers
%h - Remote host name (or IP address if enableLookups for the connector is false)
%H - Request protocol
%l - Remote logical username from identd (always returns '-')
%m - Request method (GET, POST, etc.)
%p - Local port on which this request was received. See also %{xxx}p below.
%q - Query string (prepended with a '?' if it exists)
%r - First line of the request (method and request URI)
%s - HTTP status code of the response
%S - User session ID
%t - Date and time, in Common Log Format
%u - Remote user that was authenticated (if any), else '-'
%U - Requested URL path
%v - Local server name
%D - Time taken to process the request, in millis
%T - Time taken to process the request, in seconds
%F - Time taken to commit the response, in millis
%I - Current request thread name (can compare later with stacktraces)
```

Default
```
pattern="%h %l %u %t &quot;%r&quot; %s %b" />
```

```
%h - Remote host name (or IP address if enableLookups for the connector is false)
%l - Remote logical username from identd (always returns '-')
%u - Remote user that was authenticated (if any), else '-'
%t - Date and time, in Common Log Format

%r - First line of the request (method and request URI)

%s - HTTP status code of the response
%b - Bytes sent, excluding HTTP headers, or '-' if zero
```

Proposed

```
pattern="%h %t %S &quot;%r&quot; %s %b" />
```

```
h remote host
t timestamp
S session id
r first line of request
s http status code of response
b bytes send
```


<<<<<<< HEAD
http://www.ragingcomputer.com/2014/02/removing-old-records-for-logstash-elasticsearch-kibana





https://www.ddreier.com/logstash-configuration-dissection/

To get Event Logs from the Windows boxes, I'm using nxlog-ce and (the first half of) a config that I found on the 'net somewhere
http://nxlog.org/




Logging Windows event log information to Logstash using nxlog and JSON transport


https://gist.github.com/robinsmidsrod/4215337
http://webcache.googleusercontent.com/search?q=cache:zWLISralqrkJ:https://gist.github.com/robinsmidsrod/4215337+&cd=5&hl=en&ct=clnk&gl=us


Config files used to send Windows Eventlog data with NXLOG to Logstash
https://gist.github.com/LVLAaron/8085923

Building up a logging server – OSS style!
http://girl-germs.com/?p=438


https://www.elastic.co/guide/en/logstash/master/plugins-inputs-eventlog.html


http://cppii.kcc.com/ProcessesReport?name=USTWA013&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=false&summarize=false&link#

http://cppii.kcc.com/ProcessesReport?name=&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=false&summarize=true&link=

http://cppii.kcc.com/ResponseTime?name=USTWA013&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=false&summarize=false&link=

http://cppii.kcc.com/IOReport?name=USTWA013&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=false&summarize=false&link=

http://cppii.kcc.com/MemoryReport?name=&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=true&summarize=true&link=

http://cppii.kcc.com/DiskReport?name=&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=true&summarize=true&link=

http://cppii.kcc.com/NetworkReport?name=&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=true&summarize=true&link=

http://cppii.kcc.com/ResponseTime?name=&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=false&summarize=true&link=

http://cppii.kcc.com/CorrelationReport?name=&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=false&summarize=true&link=

http://cppii.kcc.com/ProcessesReport?name=UKTCAW01&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=false&summarize=false&link

http://cppii.kcc.com/ProcessesReport?name=USTWA013&date=2015-05-18&workload=byGroup&scope=day&hour=12&statsOnly=false&summarize=false&link#

####JSON output 
- http://cppii.kcc.com/GetProcesses?report=ProcessesReport&nodeName=USTWA013&date=2015-05-18&workload=byGroup&scope=day&hour=12&listProcesses=true&statsOnly=false&summarize=false


####Download Json output
```
curl -v "cppii.kcc.com/GetProcesses?report=ProcessesReport&nodeName=USTWA013&date=2015-05-23&workload=byGroup&scope=day&hour=12&listProcesses=true&statsOnly=false&summarize=false" > C:\Logstash\Logs\prodlog.23-05-2015.json
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 165.28.44.88...
* Connected to cppii.kcc.com (165.28.44.88) port 80 (#0)
> GET /GetProcesses?report=ProcessesReport&nodeName=USTWA013&date=2015-05-23&workload=byGroup&scope=day&hour=12&listProcesses=true&statsOnly=false&summarize=false HTTP/1.1
> User-Agent: curl/7.41.0
> Host: cppii.kcc.com
> Accept: */*
>
< HTTP/1.1 200 OK
< Cache-Control: no-store
< Expires:-1
< Content-Type: text/json
< Content-Length: 1443554
< X-UA-Compatible:IE=edge
<
{ [16384 bytes data]
100 1409k  100 1409k    0     0  6024k      0 --:--:-- --:--:-- --:--:-- 6024k
* Connection #0 to host cppii.kcc.com left intact

```

### windows-powershell-getprocess https://github.com/svennergr/windows-powershell-getprocess
A NodeJS Module to get process information on Windows runtimes

Injesting Json 
http://stackoverflow.com/a/21915231

```
input {
   stdin {
   type => "stdin-type"
 }

 file {
    type => "prodlog"

    # Wildcards work, here :)
    path => [ "/root/isaac/Mylogs/testlog.log"]

    codec => json
 }
}

output {
  stdout { debug => true }
  elasticsearch { embedded => true }
}
```

JSON different fields - http://logstash.net/docs/1.4.2/filters/json
http://stackoverflow.com/questions/21907261/having-logstash-reading-json

https://www.elastic.co/guide/en/logstash/current/input-plugins.html
=======
Slides
====
1- [Logging with Logstash](files/2014-04-22_LoggingWithLogstash_BastianWidmer_CampusBern.pdf) [source](http://bastianwidmer.ch/slides/2014-04-22_LoggingWithLogstash_BastianWidmer_CampusBern.pdf)

2-  [Logstash Kibana Intro](files/logstash-kibana-intro.pdf) [Source](https://secure.trifork.com/dl/goto-berlin-2014/GOTO_Night/logstash-kibana-intro.pdf)



>>>>>>> 0f7e5f861565fb28c21a6e7dde2c99b012d3e665
