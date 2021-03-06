Syslog Output
=============

Please keep in mind that we don’t provide free support for third party systems, so this section will be just a brief introduction to how you would send syslog to external syslog collectors. If you need commercial support, please see https://www.securityonionsolutions.com.

How do I send Zeek and Wazuh logs to an external syslog collector?
------------------------------------------------------------------

Configure ``/etc/syslog-ng/syslog-ng.conf`` with a new ``destination`` to forward to your external syslog collector and then restart ``syslog-ng``.

How do I send IDS alerts to an external system?
-----------------------------------------------

2 options:

-  Edit ALL ``/etc/nsm/HOSTNAME-INTERFACE/barnyard2*.conf`` files on ALL sensors with a new ``output`` to send IDS alerts to your external systems and then restart all barnyard2 instances:

   ::

       sudo so-barnyard-restart

OR

-  On your master server (running sguild), configure ``/etc/syslog-ng/syslog-ng.conf`` with a new ``source`` to monitor ``/var/log/nsm/securityonion/sguild.log`` for ``Alert Received`` lines and a new ``destination`` to send to your external system, and then restart ``syslog-ng``. To do this modify ``/etc/syslog-ng/syslog-ng.conf`` and add the following lines:
   
This line specifies where the sguild.log file is located, and informs syslog-ng to tail the file, the program_override inserts the string sguil\_alert into the string:

::

   source s_sguil { file("/var/log/nsm/securityonion/sguild.log"
   program_override("sguil_alert")); };

This line filters on the string “Alert Received”:

::

   filter f_sguil { match("Alert Received"); };

This line tells syslog-ng to send the data read to the IP address of 10.80.4.37, via UDP to port 514:

::
   
   destination d_sguil_udp { udp("10.80.4.37" port(514)); };

This log section tells syslog-ng how to structure the previous ‘source / filter / destination’ and is what actually puts them into play:

::

   log {
   source(s_sguil);
   filter(f_sguil);
   destination(d_sguil_udp);
   };

Please note that this option requires ``set DEBUG 2`` in ``/etc/sguild/sguild.conf``.
