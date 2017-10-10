# OpenBSD

Tested on OpenBSD 6.0-6.2

Running this on OpenBSD requires some slight adjustments.

I already run Unbound on my OpenBSD hosts as the resolver for my network, so this contribution is configured to forward requests for Netflix domains to the python application running as a daemon on the localhost.

OpenBSD rcctl did not care for the . in the daemon name, so /usr/local/sbin/netflix is simply a copy of server.py

If you have set your PKG_PATH variable you should be able to install the required dependencies with the following:

`pkg_add -Uv py-twisted-names`

Pay attention to the final output about linking the python binaries.  
`/usr/local/bin/python -> /usr/local/bin/python2.7`  
**If you skip that step, the daemon will not run!**

You will need the following files from this repository:

`usr/local/sbin/netflix` the python daemon

`etc/rc.conf.local` add this to the rc.conf.local file to start the daemon with the system

`etc/rc.d/netflix` the rc script to start the daemon

`var/unbound/etc/unbound.conf` add this to your unbound.conf to include the netflix.conf file

`var/unbound/etc/netflix.conf` the configuration to forward Netflix queries to the daemon

Once all files are in their proper places (with correct ownership and permissions; reference other files in the same directory), you will need to enable and start the daemon, verify your unbound config and reload unbound.

```
rcctl enable netflix
rcctl start netflix
unbound-checkconf
unbound-control reload
```
