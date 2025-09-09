# dnsenum

**Mandatory:**

> `dnsenum --enum <domain>`

### Efficient Query

{% code overflow="wrap" %}
```bash
dnsenum --enum -f /usr/share/SecLists/Discovery/DNS/subdomain-top1million-110000.txt -r
#-r for recursive subdomain bruteforcing
```
{% endcode %}
