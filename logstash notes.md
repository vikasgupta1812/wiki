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


Logstash

- [Little Logstash Lessons - Part I: Using grok and mutate to type your data](https://www.elastic.co/blog/little-logstash-lessons-part-using-grok-mutate-type-data)
- Grok Patterns = https://github.com/elastic/logstash/blob/v1.4.2/patterns/grok-patterns
- http://logstash.net/docs/1.0.17/filters/grok

Reliance log
```
2015-05-13 00:01:37,169 [ContainerBackgroundProcessor[StandardEngine[Catalina]]] INFO com.etq.reliance.business.UserManager - Ending session for W51067
2015-05-13 00:01:37,170 [ContainerBackgroundProcessor[StandardEngine[Catalina]]] INFO com.etq.reliance.control.HttpProxyListener - W51067's session has timed out
2015-05-13 00:02:18,382 [Timer-37] INFO com.etq.reliance.timers.DataIntegrationTask - Started task: SFDC to EtQ Data Integration Task (2nd Try)
2015-05-13 00:02:19,475 [Timer-37] INFO com.etq.reliance.custom.datacenter.dataIntegration.ConnectionProfileWrapper - Data Integration [SFDC to ETQ (NA and AP)] has started on 5/13/15 12:02 AM
2015-05-13 00:02:19,493 [Timer-37] INFO com.etq.reliance.custom.datacenter.dataIntegration.ConnectionProfileWrapper - Data Integration [SFDC to ETQ (NA and AP)] has finished on 5/13/15 12:02 AM
2015-05-13 00:02:38,427 [Timer-37] INFO com.etq.reliance.custom.datacenter.dataIntegration.ConnectionProfileWrapper - Data Integration Report [SFDC to ETQ (NA and AP)] Wed May 13 00:02:38 CDT 2015

Data Integration process has successfully completed with no errors.

Number of created Reliance documents >> 0
Number of updated Reliance documents >> 0
Number of processed staging table records >> 0
Number of processed staging table records for update>> 0
Start Date >> Wed May 13 00:02:19 CDT 2015
End Date >> Wed May 13 00:02:19 CDT 2015
2015-05-13 00:02:38,440 [Timer-37] INFO com.etq.reliance.timers.DataIntegrationTask - Ended task: SFDC to EtQ Data Integration Task (2nd Try)
2015-05-13 00:03:26,148 [http-80-112] INFO com.etq.reliance.business.UserManager - New session for W63375
2015-05-13 00:03:26,148 [http-80-112] INFO com.etq.reliance.business.UserManager - License usage: 34 Users (34 Full)
2015-05-13 00:04:12,720 [Timer-34] INFO com.etq.reliance.timers.DataIntegrationTask - Started task: KCP EU SFDC to EtQ Data Integration Task (2nd one)
2015-05-13 00:04:13,818 [Timer-34] INFO com.etq.reliance.custom.datacenter.dataIntegration.ConnectionProfileWrapper - Data Integration [SFDC to ETQ (EMEA)] has started on 5/13/15 12:04 AM
2015-05-13 00:04:13,836 [Timer-34] INFO com.etq.reliance.custom.datacenter.dataIntegration.ConnectionProfileWrapper - Data Integration [SFDC to ETQ (EMEA)] has finished on 5/13/15 12:04 AM
2015-05-13 00:04:14,443 [Timer-34] INFO com.etq.reliance.custom.datacenter.dataIntegration.ConnectionProfileWrapper - Data Integration Report [SFDC to ETQ (EMEA)] Wed May 13 00:04:14 CDT 2015

Data Integration process has successfully completed with no errors.

Number of created Reliance documents >> 0
Number of updated Reliance documents >> 0
Number of processed staging table records >> 0
Number of processed staging table records for update>> 0
Start Date >> Wed May 13 00:04:13 CDT 2015
End Date >> Wed May 13 00:04:13 CDT 2015
2015-05-13 00:04:14,449 [Timer-34] INFO com.etq.reliance.timers.DataIntegrationTask - Ended task: KCP EU SFDC to EtQ Data Integration Task (2nd one)
2015-05-13 00:04:26,893 [http-80-241] INFO com.etq.reliance.business.UserManager - New session for W89231
2015-05-13 00:04:26,893 [http-80-241] INFO com.etq.reliance.business.UserManager - License usage: 35 Users (35 Full)
2015-05-13 00:04:31,653 [http-80-241] INFO com.etq.reliance.business.UserManager - New session for W53854
2015-05-13 00:04:31,653 [http-80-241] INFO com.etq.reliance.business.UserManager - License usage: 36 Users (36 Full)
```

------

http://blog.lanyonm.org/articles/2014/01/12/logstash-multiline-tomcat-log-parsing.html

Example log

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
The multiline filter is the key for Logstash to understand log events that span multiple lines. In my case, each Tomcat log entry began with a timestamp, making the timestamp the best way to detect the beginning of an event.
http://logstash.net/docs/1.3.2/filters/multiline

my grok-patterns gist: https://gist.github.com/LanyonM/8390458#file-grok-patterns-L101

```
CATALINA_DATESTAMP %{MONTH} %{MONTHDAY}, 20%{YEAR} %{HOUR}:?%{MINUTE}(?::?%{SECOND}) (?:AM|PM)
TOMCAT_DATESTAMP 20%{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{HOUR}:?%{MINUTE}(?::?%{SECOND}) %{ISO8601_TIMEZONE}
```


Using these two patterns, we are able to construct the multiline pattern to match both conversion patterns. The `negate` and `previous` mean that each line will log-line rolls into the previous lines unless the pattern is matched.

```
  if [type] == "tomcat" {
    multiline {
      patterns_dir => "/Users/lanyonm/logstash/patterns"
      pattern => "(^%{TOMCAT_DATESTAMP})|(^%{CATALINA_DATESTAMP})"
      negate => true
      what => "previous"
    }
```

The next thing to do is parse each event into its constituent parts. In the Tomcat log example above, the timestamp is followed by a logging level, classname and log message. Grok already provides some of of these patterns, so we just had to glue them together. Again, because there are two different syntaxes for a log statement, we have two patterns:


```
CATALINALOG %{CATALINA_DATESTAMP:timestamp} %{JAVACLASS:class} %{JAVALOGMESSAGE:logmessage}
TOMCATLOG %{TOMCAT_DATESTAMP:timestamp} \| %{LOGLEVEL:level} \| %{JAVACLASS:class} - %{JAVALOGMESSAGE:logmessage}
```

We see some of the filter nuance below. These two patterns can be checked against an event by specifying the match with a hash of comma-separated keys and values. The grok filter will attempt to match each pattern before failing to parse. The filter’s [match documentation](http://logstash.net/docs/1.3.2/filters/grok#match) isn’t quite perfected on this point yet. Have a look at the grok filter below:


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

Inevitably, there will be mess in your logs that doesn’t conform to your grok parser. You can choose to drop events that fail to parse by using the drop filter inside a conditional as shown on the second line above. Where do `[tags]` come from you might ask?

Tags can be applied to events at several points in the processing pipeline. For example, when the `multiline` filter successfully parses an event, it tags the event with "multiline".


The `date` filter can accept a comma separated list of timestamp patterns to match. This allows either the `CATALINA_DATESTAMP` pattern or the `TOMCAT_DATESTAMP` pattern to match the date filter and be ingested by Logstash correctly.

###Output
The output is simply an embedded Elasticsearch config as well as debugging to stdout. If you’d like to see the full config, have a look at the [gist](https://gist.github.com/LanyonM/8390458#file-logstash-java-conf).

```
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
###Grok Patterns
There’s no magic to grok patterns (unless the built-ins work for you). There are however a couple resources that can make your parsing go faster. 

First is the [Grok Debugger](http://grokdebug.herokuapp.com/). You can paste messages into the Discover tab and the Debugger will find the best matches against the built in patterns. 

Another regex assistant I use is [RegExr](http://regexr.com/). I have the native app, but the web page is nice too.

The full list of patterns shipped with Logstash can be found [on GitHub](https://github.com/logstash/logstash/tree/master/patterns), and the ones I used can be found in this Gist. If you’re not into clicking links, here are the important ones:

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

###The Kibana Dashboard

Elasticsearch and Kibana can put all logs on the same timeline.  


## File inputs 
http://logstash.net/docs/1.3.2/inputs/file

By default, each event is assumed to be one line. If you want to join lines, you'll want to use the multiline filter.

Files are followed in a manner similar to "tail -0F". File rotation is detected and handled by this input.


