---
layout: post
title: Apache Solr 9 - Tutorial 1 - Index and search 'techproducts' example data
author: Ralf Eichinger
toc: true
---

After installing Solr 9 in our previous post ["Apache Solr 9 - Installation, Configuration and Usage (on localhost)"]({% post_url topics/server/apache-solr/2022-06-12-apache-solr-9 %})
we are going to play around with example data that is provided with the standard installation under subdirectory `example` (`$SOLR_INSTALL/example`).

For reference see <https://solr.apache.org/guide/solr/latest/getting-started/tutorial-techproducts.html>.

It is assumed that Solr is installed under `/opt/solr-9.0.0` (referred to as `$SOLR_INSTALL`) with file permissions for user `solr`.

# Launch Solr in SolrCloud Mode

If Solr is still running standalone, issue the command

```
$ sudo -u solr $SOLR_INSTALL/bin/solr stop -all
```

Now start Solr in SolrCloud mode with this configuration:

* two servers/nodes
* two shards
* two replicas
* collection `techproducts` with `sample_techproducts_configs` set of configuration files

```
$ cd $SOLR_INSTALL
$ sudo -u solr bin/solr start -e cloud
*** [WARN] *** Your open file limit is currently 1024.  
 It should be set to 65000 to avoid operational disruption. 
 If you no longer wish to see this warning, set SOLR_ULIMIT_CHECKS to false in your profile or solr.in.sh

Welcome to the SolrCloud example!

This interactive session will help you launch a SolrCloud cluster on your local workstation.
To begin, how many Solr nodes would you like to run in your local cluster? (specify 1-4 nodes) [2]: 

Ok, let's start up 2 Solr nodes for your example SolrCloud cluster.
Please enter the port for node1 [8983]: 

Please enter the port for node2 [7574]: 

Creating Solr home directory /opt/solr-9.0.0/example/cloud/node1/solr
Cloning /opt/solr-9.0.0/example/cloud/node1 into
   /opt/solr-9.0.0/example/cloud/node2

Starting up Solr on port 8983 using command:
"bin/solr" start -cloud -p 8983 -s "example/cloud/node1/solr"

Warning: Available entropy is low. As a result, use of the UUIDField, SSL, or any other features that require
RNG might not work properly. To check for the amount of available entropy, use 'cat /proc/sys/kernel/random/entropy_avail'.

Waiting up to 180 seconds to see Solr running on port 8983 [|]  
Started Solr server on port 8983 (pid=50824). Happy searching!

    
Starting up Solr on port 7574 using command:
"bin/solr" start -cloud -p 7574 -s "example/cloud/node2/solr" -z localhost:9983

Warning: Available entropy is low. As a result, use of the UUIDField, SSL, or any other features that require
RNG might not work properly. To check for the amount of available entropy, use 'cat /proc/sys/kernel/random/entropy_avail'.

Waiting up to 180 seconds to see Solr running on port 7574 [|]  
Started Solr server on port 7574 (pid=51140). Happy searching!

INFO  - 2022-09-13 15:39:17.078; org.eclipse.jetty.util.log; Logging initialized @132364ms to org.eclipse.jetty.util.log.Slf4jLog
INFO  - 2022-09-13 15:39:17.151; org.apache.solr.common.cloud.ConnectionManager; Waiting up to 15000ms for client to connect to ZooKeeper
INFO  - 2022-09-13 15:39:17.169; org.apache.solr.common.cloud.ConnectionManager; zkClient has connected
INFO  - 2022-09-13 15:39:17.169; org.apache.solr.common.cloud.ConnectionManager; Client is connected to ZooKeeper
INFO  - 2022-09-13 15:39:17.178; org.apache.solr.common.cloud.ZkStateReader; Updated live nodes from ZooKeeper... (0) -> (2)
INFO  - 2022-09-13 15:39:17.192; org.apache.solr.client.solrj.impl.ZkClientClusterStateProvider; Cluster at localhost:9983 ready

Now let's create a new collection for indexing documents in your 2-node cluster.
Please provide a name for your new collection: [gettingstarted]
```

When asked for a collection name, enter `techproducts`

```
techproducts
How many shards would you like to split techproducts into? [2]

How many replicas per shard would you like to create? [2] 

Please choose a configuration for the techproducts collection, available options are:
_default or sample_techproducts_configs [_default]
```

When asked for configset, enter `sample_techproducts_configs`

```
Created collection 'techproducts' with 2 shard(s), 2 replica(s) with config-set 'techproducts'

Enabling auto soft-commits with maxTime 3 secs using the Config API

POSTing request to Config API: http://localhost:8983/solr/techproducts/config
{"set-property":{"updateHandler.autoSoftCommit.maxTime":"3000"}}
Successfully set-property updateHandler.autoSoftCommit.maxTime to 3000


SolrCloud example running, please visit: http://localhost:89 83/solr
```

The cloud should now be up and running. Browse <http://localhost:8983/solr> for the admin UI.

Solr is now be running two "nodes", one on port 7574 and one on port 8983.

There is one collection `techproducts` created, a two shard collection, each with two replicas.

Browse <http://localhost:8983/solr/#/~cloud?view=graph> to see this graphical representation of the cloud setup:

![SolrCloud graph](/assets/topics/server/apache-solr/Apache_Solr_Admin-9.0.0-cloud-graph.png)

Select Collection `techproducts` (or browse to <http://localhost:8983/solr/#/techproducts/collection-overview>) to see
sharding and replica information for this collection:

![techproducts collection overview](/assets/topics/server/apache-solr/Apache_Solr_Admin-9.0.0-techproducts-collection.png)

# Index example data into collection 'techproducts'

Until now the collection `techproducts` contains no data. We now will index example data into it.
The data we will index is in the `$SOLR_INSTALL/example/exampledocs` directory:

```
$ ls -al $SOLR_INSTALL/example/exampledocs/
insgesamt 152
drwxr-xr-x 2 solr solr  4096  5. Mai 01:00 .
drwxr-xr-x 5 solr solr  4096 13. Sep 15:37 ..
-rw-r--r-- 1 solr solr   959  5. Mai 01:00 books.csv
-rw-r--r-- 1 solr solr  1148  5. Mai 01:00 books.json
-rw-r--r-- 1 solr solr  1333  5. Mai 01:00 gb18030-example.xml
-rw-r--r-- 1 solr solr  2245  5. Mai 01:00 hd.xml
-rw-r--r-- 1 solr solr  2074  5. Mai 01:00 ipod_other.xml
-rw-r--r-- 1 solr solr  2109  5. Mai 01:00 ipod_video.xml
-rw-r--r-- 1 solr solr  2801  5. Mai 01:00 manufacturers.xml
-rw-r--r-- 1 solr solr  3090  5. Mai 01:00 mem.xml
-rw-r--r-- 1 solr solr  2156  5. Mai 01:00 money.xml
-rw-r--r-- 1 solr solr  1402  5. Mai 01:00 monitor2.xml
-rw-r--r-- 1 solr solr  1420  5. Mai 01:00 monitor.xml
-rw-r--r-- 1 solr solr   178  5. Mai 01:00 more_books.jsonl
-rw-r--r-- 1 solr solr  1976  5. Mai 01:00 mp500.xml
-rw-r--r-- 1 solr solr 43608  5. Mai 01:00 post.jar
-rw-r--r-- 1 solr solr   235  5. Mai 01:00 sample.html
-rw-r--r-- 1 solr solr  1684  5. Mai 01:00 sd500.xml
-rw-r--r-- 1 solr solr 21052  5. Mai 01:00 solr-word.pdf
-rw-r--r-- 1 solr solr  1810  5. Mai 01:00 solr.xml
-rwxr-xr-x 1 solr solr  3742  5. Mai 01:00 test_utf8.sh
-rw-r--r-- 1 solr solr  1835  5. Mai 01:00 utf8-example.xml
-rw-r--r-- 1 solr solr  2697  5. Mai 01:00 vidcard.xml
```

The documents are in different document formats (JSON, CSV, etc.) but can be indexed all at once issuing this command:

```
$ cd $SOLR_INSTALL
$ sudo -u solr bin/post -c techproducts example/exampledocs/*
java -classpath /opt/solr-9.0.0/server/solr-webapp/webapp/WEB-INF/lib/solr-core-9.0.0.jar -Dauto=yes -Dc=techproducts -Ddata=files org.apache.solr.util.SimplePostTool example/exampledocs/books.csv example/exampledocs/books.json example/exampledocs/gb18030-example.xml example/exampledocs/hd.xml example/exampledocs/ipod_other.xml example/exampledocs/ipod_video.xml example/exampledocs/manufacturers.xml example/exampledocs/mem.xml example/exampledocs/money.xml example/exampledocs/monitor2.xml example/exampledocs/monitor.xml example/exampledocs/more_books.jsonl example/exampledocs/mp500.xml example/exampledocs/post.jar example/exampledocs/sample.html example/exampledocs/sd500.xml example/exampledocs/solr-word.pdf example/exampledocs/solr.xml example/exampledocs/test_utf8.sh example/exampledocs/utf8-example.xml example/exampledocs/vidcard.xml
SimplePostTool version 5.0.0
Posting files to [base] url http://localhost:8983/solr/techproducts/update...
Entering auto mode. File endings considered are xml,json,jsonl,csv,pdf,doc,docx,ppt,pptx,xls,xlsx,odt,odp,ods,ott,otp,ots,rtf,htm,html,txt,log
POSTing file books.csv (text/csv) to [base]
POSTing file books.json (application/json) to [base]/json/docs
POSTing file gb18030-example.xml (application/xml) to [base]
POSTing file hd.xml (application/xml) to [base]
POSTing file ipod_other.xml (application/xml) to [base]
POSTing file ipod_video.xml (application/xml) to [base]
POSTing file manufacturers.xml (application/xml) to [base]
POSTing file mem.xml (application/xml) to [base]
POSTing file money.xml (application/xml) to [base]
POSTing file monitor2.xml (application/xml) to [base]
POSTing file monitor.xml (application/xml) to [base]
POSTing file more_books.jsonl (application/json) to [base]/json/docs
POSTing file mp500.xml (application/xml) to [base]
POSTing file post.jar (application/octet-stream) to [base]/extract
POSTing file sample.html (text/html) to [base]/extract
POSTing file sd500.xml (application/xml) to [base]
POSTing file solr-word.pdf (application/pdf) to [base]/extract
POSTing file solr.xml (application/xml) to [base]
POSTing file test_utf8.sh (application/octet-stream) to [base]/extract
POSTing file utf8-example.xml (application/xml) to [base]
POSTing file vidcard.xml (application/xml) to [base]
21 files indexed.
COMMITting Solr index changes to http://localhost:8983/solr/techproducts/update...
Time spent: 0:00:02.969
```

Now the collection `techproducts` is filled with data and we can start searching!

# Basic Searching

Solr can be queried using

* HTTP-requests (REST clients like curl, wget, browser etc.)
* native clients in many programming languages (see <https://solr.apache.org/guide/solr/latest/deployment-guide/client-apis.html>)
* Solr Admin UI

For now we will illustrate the usage of HTTP requests and the Solr Admin UI.

## Searching using Solr Admin UI

To issue a query using the GUI browse to <http://localhost:8983/solr/#/techproducts/query>.
By querying without a search term (just click on button `Execute Query`) you will get the first 10 rows of all (52) existing documents (rows is limited to 10):

![techproducts empty query](/assets/topics/server/apache-solr/Apache_Solr_Admin-9.0.0-techproducts-empty-query.png)

## Searching using HTTP request

The URL sent by the Admin UI to Solr is shown in light grey near the top right of the above screenshot:

```
http://localhost:8983/solr/techproducts/select?indent=true&q.op=OR&q=*%3A*
```

If you issue this request e.g. using curl you get the same result as raw JSON response.

```
$ curl "http://localhost:8983/solr/techproducts/select?indent=true&q.op=OR&q=*%3A*"
```

(You can use the escaped param `q=*%3A*` or `q=\*:*`, same result but the latter one being better readable.)

The query param `q` is set to `*:*` to request all documents in the index.

The query param `indent` is set to `true` to indent response JSON nicely.

The results are limited to 10 (default), but we can change this by using parameter `rows`:

```
$ curl "http://localhost:8983/solr/techproducts/select?indent=true&q.op=OR&q=\*:*&rows=1"
{
  "responseHeader":{
    "zkConnected":true,
    "status":0,
    "QTime":12,
    "params":{
      "q":"\\*:*",
      "indent":"true",
      "q.op":"OR",
      "rows":"1"}},
  "response":{"numFound":52,"start":0,"maxScore":1.0,"numFoundExact":true,"docs":[
      {
        "id":"0812521390",
        "cat":["book"],
        "name":"The Black Company",
        "price":6.99,
        "price_c":"6.99,USD",
        "inStock":false,
        "author":"Glen Cook",
        "author_s":"Glen Cook",
        "series_t":"The Chronicles of The Black Company",
        "sequence_i":1,
        "genre_s":"fantasy",
        "_version_":1743864806832603136,
        "price_c____l_ns":699,
        "name_exact":"The Black Company"}]
  }}
```

... limiting rows to 1.

## Search for a single term

See <https://solr.apache.org/guide/solr/latest/getting-started/tutorial-techproducts.html#search-for-a-single-term>

As before we issue a single search term using param `q`, e.g. searching for "foundation" (limit set to 1 row):

```
$ curl "http://localhost:8983/solr/techproducts/select?q=foundation&rows=1"
{
  "responseHeader":{
    "zkConnected":true,
    "status":0,
    "QTime":10,
    "params":{
      "q":"foundation",
      "rows":"1"}},
  "response":{"numFound":5,"start":0,"maxScore":1.2758882,"numFoundExact":true,"docs":[
      {
        "id":"0553293354",
        "cat":["book"],
        "name":"Foundation",
        "price":7.99,
        "price_c":"7.99,USD",
        "inStock":true,
        "author":"Isaac Asimov",
        "author_s":"Isaac Asimov",
        "series_t":"Foundation Novels",
        "sequence_i":1,
        "genre_s":"scifi",
        "_version_":1743864806821068800,
        "price_c____l_ns":799,
        "name_exact":"Foundation"}]
  }}
```

The `responseHeader` element contains the issued params you set in the request.

The `response` element contains informations about the results (total found documents = `numFound` is 5) and the found documents in subelement `docs` as list of documents.

By default the documents we got back include all the fields for each document that were indexed. To restrict the fields in the response, you can use the param `fl`,
which takes a comma-separated list of wanted field names, e.g.

```
$ curl "http://localhost:8983/solr/techproducts/select?q=foundation&fl=id,name,author,price"
{
  "responseHeader":{
    "zkConnected":true,
    "status":0,
    "QTime":17,
    "params":{
      "q":"foundation",
      "fl":"id,name,author,price"}},
  "response":{"numFound":5,"start":0,"maxScore":1.2758882,"numFoundExact":true,"docs":[
      {
        "id":"0553293354",
        "name":"Foundation",
        "price":7.99,
        "author":"Isaac Asimov"},
      {
        "id":"UTF8TEST",
        "name":"Test with some UTF-8 encoded characters",
        "price":0.0},
      {
        "id":"SOLR1000",
        "name":"Solr, the Enterprise Search Server",
        "price":0.0},
      {
        "id":"/opt/solr-9.0.0/example/exampledocs/post.jar"},
      {
        "id":"/opt/solr-9.0.0/example/exampledocs/test_utf8.sh"}]
  }}
```

to get all 5 results with requested fields "id, name, author, price".


# Velocity UI

https://www.mail-archive.com/issues@lucene.apache.org/msg38949.html

https://github.com/erikhatcher/solr-velocity