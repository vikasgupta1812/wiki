Elasticsearch 
====

**shard** A shard is a single Lucene instance. It is a low-level “worker” unit which is managed automatically by elasticsearch. An index is a **logical namespace** which points to **primary** and **replica shards**. Other than defining the number of primary and replica shards that an index should have, you never need to refer to shards directly. Instead, your code should deal only with an index. Elasticsearch distributes shards amongst all nodes in the cluster, and can move shards automatically from one node to another in the case of node failure, or the addition of new nodes.


**primary shard** Each document is stored in a single primary shard. When you index a document, it is indexed first on the primary shard, then on all replicas of the primary shard. By default, an index has **5 primary shards**. You can specify fewer or more primary shards to scale the number of documents that your index can handle. You cannot change the number of primary shards in an index, once the index is created. See also routing

**replica shard** Each primary shard can have zero or more **replicas**. A replica is a copy of the primary shard, and has two purposes:

1- **increase failover**: a replica shard can be promoted to a primary shard if the primary fails
2- **increase performance**: get and search requests can be handled by primary or replica shards. By default, each primary shard has one replica, but the number of replicas can be changed dynamically on an existing index. A replica shard will never be started on the same node as its primary shard.

http://stackoverflow.com/questions/992988/what-is-sharding-and-why-is-it-important

> Horizontal partitioning is a design principle whereby rows of a database table are held separately, rather than splitting by columns (as for normalization). Each partition forms part of a shard, which may in turn be located on a separate database server or physical location. The advantage is the number of rows in each table is reduced (this reduces index size, thus improves search performance). If the sharding is based on some real-world aspect of the data (e.g. European customers vs. American customers) then it may be possible to infer the appropriate shard membership easily and automatically, and query only the relevant shard.

Some more information about sharding:

> Firstly, each database server is identical, having the same table structure. Secondly, the data records are logically split up in a sharded database. Unlike the partitioned database, each complete data record exists in only one shard (unless there's mirroring for backup/redundancy) with all CRUD operations performed just in that database. You may not like the terminology used, but this does represent a different way of organizing a logical database into smaller parts.


**Shards and replicas in Elasticsearch**
http://stackoverflow.com/questions/15694724/shards-and-replicas-in-elasticsearch

I am trying to understand what shard and replica is in Elasticsearch, but I don't manage to understand it. If I download Elasticsearch and run the script, then from what I know I have started a cluster with a single node. Now this node (my PC) have 5 shards (?) and some replicas (?).

What are they, do I have 5 duplicates of the index? If so why? I could need some explanation.


When you download elasticsearch and start it up you create an elasticsearch node which tries to join an existing cluster if available or creates a new one. Let's say you created your own new cluster with a single node, the one that you just started up. We have no data, therefore we need to create an index.

When you create an index (an index is automatically created when you index the first document as well) you can define how many shards it will be composed of. If you don't specify a number it will have the default number of shards: 5 primaries. What does it mean?

It means that elasticsearch will create 5 primary shards that will contain your data:

```
 ____    ____    ____    ____    ____
| 1  |  | 2  |  | 3  |  | 4  |  | 5  |
|____|  |____|  |____|  |____|  |____|
```
Every time you index a document elasticsearch will decide which primary shard is supposed to hold that document and will index it there. Primary shards are not copy of the data, they are the data! Having multiple shards does help taking advantage of parallel processing on a single machine, but the whole point is that if we start another elasticsearch instance on the same cluster, the shards will be distributed in an even way over the cluster.

Node 1 will then hold for example only three shards:

```
 ____    ____    ____ 
| 1  |  | 2  |  | 3  |
|____|  |____|  |____|
```
Since the remaining two shards have been moved to the newly started node:

```
 ____    ____
| 4  |  | 5  |
|____|  |____|
```
Why does this happen? Because elasticsearch is a `distributed search engine` and this way you can make use of multiple nodes/machines to manage big amounts of data.

Every `elasticsearch index` is composed of at least one primary shard, since that's where the data is stored. Every shard comes at a cost though, therefore if you have a single node and no foreseeable growth, just stick with a single primary shard.

Another type of shard is `replica`. The default is 1, meaning that every primary shard will be copied to another shard that will contain the same data. Replicas are used to increase search performance and for fail-over. A replica shard is never going to be allocated on the same node where the related primary is (it would pretty much be like putting a backup on the same disk as the original data).

Back to our example, with 1 replica we'll have the whole index on each node, since 3 replica shards will be allocated on the first node and they will contain exactly the same data as the primaries on the second node:

```
 ____    ____    ____    ____    ____
| 1  |  | 2  |  | 3  |  | 4R |  | 5R |
|____|  |____|  |____|  |____|  |____|
```
Same for the second node, which will contain a copy of the primary shards on the first node:

```
 ____    ____    ____    ____    ____
| 1R |  | 2R |  | 3R |  | 4  |  | 5  |
|____|  |____|  |____|  |____|  |____|
```
With a setup like this, if a node goes down you still have the whole index. The replica shards will automatically become primaries and the cluster will work properly despite the node failure, as follows:

```
 ____    ____    ____    ____    ____
| 1  |  | 2  |  | 3  |  | 4  |  | 5  |
|____|  |____|  |____|  |____|  |____|
```
Since you have "number_of_replicas":1, the replicas cannot be assigned anymore as they are never allocated on the same node where their primary is. That's why you'll have 5 unassigned shards, the replicas, and the cluster status will be YELLOW instead of GREEN. No data loss, but it could be better as some shards cannot be assigned.

As soon as the node that had left is back up, it'll join the cluster again and the replicas will be assigned again. The existing shard on the second node can be loaded but they need to be synchronized with the other shards, as write operations most likely happened while the node was down. At the end of this operation the cluster status will become GREEN.

Hope this clarifies things for you.