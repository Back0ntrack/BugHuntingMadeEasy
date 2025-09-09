# theHarvester

**Mandatory: `theHarvester -d [DOMAIN]`**

**OPTIONS:**

* `-s` or `--shodan` : Use Shodan to query discovered hosts.
* `n` or `--dns-lookup`: Enable DNS server lookup.
* `-c` or `--dns-brute`: Perform a DNS brute force on the domain.
* `-b` or `--source`: Search engine to be used.
* `-f <filename>`: Store results in form of xml or JSON.

> **To add API keys to `theHarvester` navigate to `/etc/theHarvester` where you will find `api_keys.yml` and add keys to it.**

**Added API keys**

{% code overflow="wrap" %}
```bash
shodan, SecurityTrails, censys.io, binaryedge, bevigil, virustotal, projectdiscovery, hunter
```
{% endcode %}

**Free to Use module**

{% code overflow="wrap" %}
```
anubis,baidu,certspotter,crtsh,dnsdumpster,duckduckgo,hackertarget,otx,rapiddns,sitedossier,subdomaincenter,subdomainfinderc99,threatminer,urlscan,yahoo
```
{% endcode %}

General use: `theHarvester -d <domain> -s -n -c -b all`
