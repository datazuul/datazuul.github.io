---
layout: post
title: Apache Solr 9 - Velocity UI ("Solritas")
author: Ralf Eichinger
toc: true
---


After installing Solr 9 (see post ["Apache Solr 9 - Installation, Configuration and Usage (on localhost)"]({% post_url topics/server/apache-solr/2022-06-12-apache-solr-9 %}))
and playing around with "techproducts" example data we are going to install the user UI called "Solritas" that is based on Velocity.

Old screenshot of outdated Solr documentation:

![Solritas screenshot](/assets/topics/server/apache-solr/solritas-techproducts_browse.png)

Since Solr 9 this is [no longer bundled](https://www.mail-archive.com/issues@lucene.apache.org/msg38949.html) within the solr installation package but maintained separately on GitHub by its developer Eric Hatcher: <https://github.com/erikhatcher/solr-velocity>

# Specification

* Homepage: <https://github.com/erikhatcher/solr-velocity>
* Version 0.9.5

# Installation

## Start Solr with package manager enabled

Stop cloud:

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr stop -all
```

Start (techproducts-cloud example) again with packages enabled (see <https://solr.apache.org/guide/solr/latest/configuration-guide/package-manager.html#enable-the-package-manager>):

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr start -e cloud -Denable.packages=true
*** [WARN] *** Your open file limit is currently 1024.  
 It should be set to 65000 to avoid operational disruption. 
 If you no longer wish to see this warning, set SOLR_ULIMIT_CHECKS to false in your profile or solr.in.sh

Welcome to the SolrCloud example!

This interactive session will help you launch a SolrCloud cluster on your local workstation.
To begin, how many Solr nodes would you like to run in your local cluster? (specify 1-4 nodes) [2]: 

Ok, let's start up 2 Solr nodes for your example SolrCloud cluster.
Please enter the port for node1 [8983]: 

Please enter the port for node2 [7574]: 

Solr home directory /opt/solr-9.0.0/example/cloud/node1/solr already exists.
/opt/solr-9.0.0/example/cloud/node2 already exists.

Starting up Solr on port 8983 using command:
"/opt/solr-9.0.0/bin/solr" start -cloud -p 8983 -s "/opt/solr-9.0.0/example/cloud/node1/solr" -Denable.packages=true

Warning: Available entropy is low. As a result, use of the UUIDField, SSL, or any other features that require
RNG might not work properly. To check for the amount of available entropy, use 'cat /proc/sys/kernel/random/entropy_avail'.

Waiting up to 180 seconds to see Solr running on port 8983 [|]  
Started Solr server on port 8983 (pid=39275). Happy searching!

    
Starting up Solr on port 7574 using command:
"/opt/solr-9.0.0/bin/solr" start -cloud -p 7574 -s "/opt/solr-9.0.0/example/cloud/node2/solr" -z localhost:9983 -Denable.packages=true

Warning: Available entropy is low. As a result, use of the UUIDField, SSL, or any other features that require
RNG might not work properly. To check for the amount of available entropy, use 'cat /proc/sys/kernel/random/entropy_avail'.

Waiting up to 180 seconds to see Solr running on port 7574 [|]  
Started Solr server on port 7574 (pid=39516). Happy searching!

INFO  - 2022-09-14 12:34:19.583; org.eclipse.jetty.util.log; Logging initialized @24156ms to org.eclipse.jetty.util.log.Slf4jLog
INFO  - 2022-09-14 12:34:19.706; org.apache.solr.common.cloud.ConnectionManager; Waiting up to 15000ms for client to connect to ZooKeeper
INFO  - 2022-09-14 12:34:23.108; org.apache.solr.common.cloud.ConnectionManager; zkClient has connected
INFO  - 2022-09-14 12:34:23.109; org.apache.solr.common.cloud.ConnectionManager; Client is connected to ZooKeeper
INFO  - 2022-09-14 12:34:23.125; org.apache.solr.common.cloud.ZkStateReader; Updated live nodes from ZooKeeper... (0) -> (2)
INFO  - 2022-09-14 12:34:23.139; org.apache.solr.client.solrj.impl.ZkClientClusterStateProvider; Cluster at localhost:9983 ready

Now let's create a new collection for indexing documents in your 2-node cluster.
Please provide a name for your new collection: [gettingstarted] 
techproducts

Collection 'techproducts' already exists!
Do you want to re-use the existing collection or create a new one? Enter 1 to reuse, 2 to create new [1]: 
1

Enabling auto soft-commits with maxTime 3 secs using the Config API

POSTing request to Config API: http://localhost:8983/solr/techproducts/config
{"set-property":{"updateHandler.autoSoftCommit.maxTime":"3000"}}
Successfully set-property updateHandler.autoSoftCommit.maxTime to 3000


SolrCloud example running, please visit: http://localhost:8983/solr
```

## Add solritas repository as trusted package repo

See <https://solr.apache.org/guide/solr/latest/configuration-guide/package-manager.html#add-trusted-repositories>.

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr package add-repo soritas https://raw.githubusercontent.com/erikhatcher/solritas/master/repo
Added repository: soritas
```

## List installed packages and install soritas package

See <https://solr.apache.org/guide/solr/latest/configuration-guide/package-manager.html#listing-and-installing-packages>

List installed packages:

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr package list-installed
Installed packages:
-----
```

List available packages:

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr package list-available
Available packages:
-----
solritas 		templated response writer
	Version: 0.9.5
```

Install solritas package:

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr package install solritas
Posting manifest...
Posting artifacts...
Executing Package API to register this package...
Response: {"responseHeader":{
    "status":0,
    "QTime":110}}
solritas installed.
```

## Deploy solritas package to techproducts collection

After package has installed you must deploy it to a collection so that it can be used.
(original readme states to also add 'title' as primary field?: bin/solr package deploy -y solritas -collections my_data -p DF=title)

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr package deploy -y solritas -collections techproducts
xecuting {"add-queryresponsewriter":{"name":"solritas","class":"solritas:org.apache.solr.response.VelocityResponseWriter"}} for path:/api/collections/techproducts/config
Executing {"add-requesthandler":{"name":"/browse","class":"solr.SearchHandler","defaults":{"debug":"true","df":"_text_"},"useParams":"query,facets,solritas,browse"}} for path:/api/collections/techproducts/config
Executing {"set":{"query":{"defType":"edismax","q.alt":"*:*","rows":"10","fl":"*,score"}}} for path:/api/collections/techproducts/config/params
Executing {"set":{"facets":{"facet":"on","facet.mincount":"1"}}} for path:/api/collections/techproducts/config/params
Executing {"set":{"solritas":{"wt":"solritas","v.template":"browse","v.layout":"layout"}}} for path:/api/collections/techproducts/config/params
Executing http://localhost:7574/api/collections/techproducts/config/queryResponseWriter?componentName=solritas&meta=true for collection:techproducts
{
  "responseHeader":{
    "status":0,
    "QTime":2},
  "config":{"queryResponseWriter":{"solritas":{
        "name":"solritas",
        "class":"solritas:org.apache.solr.response.VelocityResponseWriter",
        "_packageinfo_":{
          "package":"solritas",
          "version":"0.9.5",
          "files":["/package/solritas/0.9.5/solritas-0.9.5.jar",
            "/package/solritas/0.9.5/velocity-engine-core-2.1.jar",
            "/package/solritas/0.9.5/velocity-tools-generic-3.0.jar"],
          "manifest":"/package/solritas/0.9.5/manifest.json",
          "manifestSHA512":"1684cc8ed7ed58d08868b5a92cbfa399510a36a461ec00f81dd15f7ab59902869f933c4cbf1863a53818b5167dfde6f781ef977687310e4eb75186f464d3fe4d"}}}}}

Actual: 0.9.5, expected: 0.9.5
Executing http://localhost:7574/api/collections/techproducts/config/requestHandler?componentName=/browse&meta=true for collection:techproducts
{
  "responseHeader":{
    "status":0,
    "QTime":1},
  "config":{"requestHandler":{"/browse":{
        "name":"/browse",
        "class":"solr.SearchHandler",
        "defaults":{
          "debug":"true",
          "df":"_text_"},
        "useParams":"query,facets,solritas,browse"}}}}

Actual: /browse, expected: /browse
Executing http://localhost:7574/api/collections/techproducts/config/params/query for collection:techproducts
{
  "responseHeader":{
    "status":0,
    "QTime":0},
  "response":{
    "znodeVersion":6,
    "params":{"query":{
        "defType":"edismax",
        "q.alt":"*:*",
        "rows":"10",
        "fl":"*,score",
        "":{"v":0}}}}}

Actual: edismax, expected: edismax
Executing http://localhost:7574/api/collections/techproducts/config/params/facets for collection:techproducts
{
  "responseHeader":{
    "status":0,
    "QTime":0},
  "response":{
    "znodeVersion":6,
    "params":{"facets":{
        "facet":"on",
        "facet.mincount":"1",
        "":{"v":0}}}}}

Actual: on, expected: on
Executing http://localhost:7574/api/collections/techproducts/config/params/solritas for collection:techproducts
{
  "responseHeader":{
    "status":0,
    "QTime":0},
  "response":{
    "znodeVersion":6,
    "params":{"solritas":{
        "wt":"solritas",
        "v.template":"browse",
        "v.layout":"layout",
        "":{"v":0}}}}}

Actual: solritas, expected: solritas
Deployed on [techproducts] and verified package: solritas, version: 0.9.5
Deployment successful
```

Unfortunately, we get a `java.security.AccessControlException` when accessing the UI with <http://localhost:8983/solr/techproducts/browse>

Even compiling source code with recent Java 17 (our Solr is running on) and copying `solritas-0.9.5.jar` to Solr locations:

```
$ sudo -u solr cp ~/development/github.com/erikhatcher/solr-velocity/target/solritas-0.9.5.jar example/cloud/node1/solr/filestore/package/solritas/0.9.5
$ sudo -u solr cp ~/development/github.com/erikhatcher/solr-velocity/target/solritas-0.9.5.jar example/cloud/node2/solr/filestore/package/solritas/0.9.5
```

did not bring the UI up. Only JSON is returned.

In Solr logs we get these errors:

```
14.9.2022, 13:01:06
ERROR true
PackageStoreAPI
Error validating package artifact

14.9.2022, 13:01:06
ERROR false
PackageLoader
package could not be loaded null

14.9.2022, 13:01:06
ERROR false
PackageLoader
latest version: null

...

14.9.2022, 13:01:07
ERROR false
techproducts_shard1_replica_n6
PackagePluginHolder
No latest version available for package : solritas

14.9.2022, 13:01:07
ERROR false
techproducts_shard2_replica_n4
PackagePluginHolder
No latest version available for package : solritas

14.9.2022, 13:01:25
ERROR false
PackagePluginHolder
No latest version available for package : solritas

14.9.2022, 13:01:25
ERROR false
PackagePluginHolder
No latest version available for package : solritas
```

So package must be made "validatable"....

No further analysis done.