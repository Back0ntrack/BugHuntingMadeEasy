# Katana

## Input Options

* `-u`/`-list`: target url/list to crawl



## Configuration

* `-d`: maximum depth to crawl. _\[default 3, useful 5]_
* `-jc`: It executes JavaScript in a headless browser to discover dynamically generated URLs that appear only after JS runs on the page.&#x20;
* `-jsl`: Find `.js` files linked on the target, then reads their content to extract URLs, API endpoints etc.&#x20;
* `-kf all`: crawl all known files such as `robots.txt`, `sitemap.xml`

> Using `katana -u https://target.com -jc` executes JavaScript in a headless browser to find dynamic URLs like `/dashboard/data` or `/user/profile` that load only after the page runs JS. On the other hand, `katana -u https://target.com -jsl` scans/reads linked JavaScript files and extracts hardcoded paths like `/api/v1/login` or `/admin/config`. This helps uncover both runtime and static endpoints.

### Crawling API files

{% code overflow="wrap" %}
```bash
katana -u https://target.com -d 5 -mdc 'contains(endpoint,"api")' -jc -o api_endpoints.txt
```
{% endcode %}

{% code overflow="wrap" %}
```bash
// Don't know but try
katana -u https://target.com -jc -fx -xhr -em js,json -sf qurl,key,value -o api_endpoints.txt
```
{% endcode %}

### Crawling JS files

{% code overflow="wrap" %}
```bash
katana -list {domains.txt} -d 5 -jc -jsl | grep ".js$"  | uniq | sort
```
{% endcode %}
