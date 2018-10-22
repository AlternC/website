## AlternC and the sub-domains paradigm

AlternC uses a templating system to create DNS entries and HTTP(s) Vhosts all at the same time.

Each domain name in AlternC is stored in the "domaines" table, along with a few fields : 

* gesmx is 1 if Postfix believes it is managing the emails (MX) of this domain name
* gesdns is 1 to tell Bind9 DNS server that we are the authoritative server for this domain name (zone)
* dns_action is "OK" when the domain is okay, and one can set it to "UPDATE" to force the zone to be recomputed by AlternC cron.

