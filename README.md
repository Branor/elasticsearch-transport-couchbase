Couchbase Transport Plugin for ElasticSearch
=================================================

This plugin makes your ElasticSearch node appear like a Couchbase Server node.  After installation you can use the Cross-datacenter Replication (XDCR) feature of Couchbase Server 2.0 to transfer data continuously.

Installation
============

In order to install the plugin, download the zip file from the Downloads page, then run `bin/plugin -install transport-couchbase -url file:///tmp/elasticsearch-transport-couchbase-1.0.0-SNAPSHOT-plugin.zip`

    +---------------------------------------------------+
    |  Plugin        |  Couchbase    | ElasticSearch    |
    +---------------------------------------------------+
    | master         |  2.0 (1464)   | 0.19.9           |
    +---------------------------------------------------+
    | 1.0.0-beta     |  2.0 (1554)   | 0.19.9           |
    +---------------------------------------------------+
    
Configuration
=============

- couchbase.port - the port the plugin will listen on, default 9091
- couchbase.username - the username for HTTP basic auth, default Administrator
- couchbase.password - the password for HTTP basic auth, no default

Usage
=====

Preparing ElasticSearch

1. Install the plugin on each node in your cluster.
2. Install the Couchbase template
    curl -XPUT http://localhost:9200/_template/couchbase -d @plugins/transport-couchbase/couchbase_template.json
3. Create an ElasticSearch index to store the data from Couchbase (ie. default)

Preparing Couchbase

1. Navigate to the Couchbase Server admin interface.
2. Select the Replications tab.
3. Press the button labeled "Create Cluster Reference"
4. Choose a name for your ElasticSearch cluster
5. In the IP/Hostname and field provide an address and port of one of the nodes in your ES cluster (127.0.0.1:9091)
6. Enter the Username and Password corresponding to your "couchbase.username" and "couchbase.password" settings in ElasticSearch
7. Press the "Save" button

Starting Data Transfer

1. Press the button labeled "Create Replication"
2. Select the bucket from source cluster you wish to send to ElasticSearch
3. Next select the cluster you defined in step 4.
4. Type in the name of the ElasticSearch index you wish to store the data in.  This index must already exist.

Building
========

This module is built using maven.  It depends on another project which is not in any public maven repositories, see https://github.com/couchbaselabs/couchbase-capi-server and run `mvn install` on that first.

Then in this project run

    mvn package
    
The final plugin package will be in the target/releases folder.
