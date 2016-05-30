How to install ELK Stack on Mac
====



```
$brew search logstash kibana elasticsearch
logstash

$brew search kibana elasticsearch
kibana

$brew search elasticsearch
elasticsearch
homebrew/versions/elasticsearch-0.20		   homebrew/versions/elasticsearch12
homebrew/versions/elasticsearch090		       homebrew/versions/elasticsearch13
homebrew/versions/elasticsearch11		       homebrew/versions/elasticsearch14
```




```
logstash: stable 1.5.2
Tool for managing events and logs
https://www.elastic.co/products/logstash
/usr/local/Cellar/logstash/1.5.2 (9290 files, 160M) *
  Built from source
From: https://github.com/Homebrew/homebrew/blob/master/Library/Formula/logstash.rb
==> Caveats
Logstash 1.5 emits an unhelpful error if you try to start it without config.
Please read the getting started guide located at:
https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html
```

```
kibana: stable 4.1.0 (bottled), HEAD
Visualization tool for elasticsearch
https://www.elastic.co/products/kibana
/usr/local/Cellar/kibana/4.1.0 (3066 files, 28M) *
  Poured from bottle
From: https://github.com/Homebrew/homebrew/blob/master/Library/Formula/kibana.rb
==> Dependencies
Required: node ✔
==> Caveats

To have launchd start kibana at login:
    ln -sfv /usr/local/opt/kibana/*.plist ~/Library/LaunchAgents
Then to load kibana now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.kibana.plist
Or, if you don't want/need launchctl, you can just run:
    kibana
```

```
elasticsearch: stable 1.6.0, HEAD
Distributed real-time search & analytics engine for the cloud
https://www.elastic.co/products/elasticsearch
/usr/local/Cellar/elasticsearch/1.6.0 (34 files, 30M) *
  Built from source
From: https://github.com/Homebrew/homebrew/blob/master/Library/Formula/elasticsearch.rb
==> Caveats
Data:    /usr/local/var/elasticsearch/elasticsearch_vikas/
Logs:    /usr/local/var/log/elasticsearch/elasticsearch_vikas.log
Plugins: /usr/local/var/lib/elasticsearch/plugins/
Config:  /usr/local/etc/elasticsearch/


To have launchd start elasticsearch at login:
    ln -sfv /usr/local/opt/elasticsearch/*.plist ~/Library/LaunchAgents
Then to load elasticsearch now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.elasticsearch.plist
Or, if you don't want/need launchctl, you can just run:
    elasticsearch --config=/usr/local/opt/elasticsearch/config/elasticsearch.yml
```
