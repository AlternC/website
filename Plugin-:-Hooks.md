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
* dns_reconfig
* dns_reload_zone
* web_reload
* dns_reload

# hook on alternc class

Some classes provide hook to allow plugin to interact with them.
We can find hook available on each class with **invoke** method.
