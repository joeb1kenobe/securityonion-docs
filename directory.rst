Directory Structure
===================

/nsm Directory Structure
------------------------

/nsm
----

Backup, Zeek, sensor (if configured as sensor), and server (if configured
as server) data.

/nsm/bro
--------

Zeek IDS logs.

/nsm/elasticsearch
------------------

Elasticsearch data.

/nsm/sensor\_data
-----------------

Sensor data including IDS alerts and full pcap organized by sensor name ($HOSTNAME-$INTERFACE).

/nsm/server\_data
-----------------

Server data including IDS rulesets.
