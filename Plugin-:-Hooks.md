# alternc.install

Alternc to override behavior provide an **alternc.install** script to execute to any installation/update alternc appliance.
This script provide some hook to allow plugin to install its configuration.

Files must be set in **/usr/lib/alternc/install.d**. Plugin can catch these events : 
* startup
* upgrade
* templates
* apache2
* before-reload
* end

# Update domain

Update domain script provide also some hook  : 

dns_reconfig
* When a domain is deleted and after rndc reconfig
* when a domain is added and after rndc reconfig

dns_reload_zone
* When a domain is updated, before update bind

web_reload
* 

dns_reload
* 

# hook on alternc class

Some classes provide hook to allow plugin to interact with them.
We can find hook available on each class with **invoke** method.
