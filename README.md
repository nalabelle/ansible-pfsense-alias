ansible-pfsense-alias
=========

EXPERIMENTAL!

A role that will add host aliases in [pfsense](https://www.pfsense.org/) running unbound for DNS.

DNS setup using [Traefik](https://traefik.io/) for docker got me pretty down, as I automatically provision new containers in ansible. I was looking for a good way to automatically point to the subdomains that it was setting up for my containers. I couldn't use wildcard DNS as it's actually running on my local internal domain. The only other good option I could find was to add labels to each container and set up some pretty annoying path stuff.

I did know that I could add host aliases to my docker host in unbound/pfsense and those seemed to work, so I was wondering if I could find some way to automatically pass the new Traefik hosts' names to that interface somehow. I went down a terrible rabbit hole and ended up with this.

It uses ansible templating to output a php script to a local tempfile on your drive, copy it up to pfsense under `/etc/phpshellsessions/`, and run `pfSsh.php playback` on it. It's pretty hacky, but it seems to work for now...


Requirements
------------

pfsense
a will to risk your router config

Role Variables
--------------

    # hostname and domain to add the aliases on
    # this should already be set up as a host override in DNS Resolver config.
    host: www
    domain: example.com
    # stick a list of the aliases you want to add to it in here
    aliases:
      - demo

Dependencies
------------

None

Example Playbook
----------------

    - hosts: pfsense
      roles:
        - ansible-pfsense-alias

License
-------

MIT

Author Information
------------------

https://github.com/nalabelle
