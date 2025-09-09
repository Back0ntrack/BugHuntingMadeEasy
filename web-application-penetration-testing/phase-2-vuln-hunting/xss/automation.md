# Automation

{% code overflow="wrap" %}
```bash
echo 'https://target.com' | gau | gf xss | uro | tee xss_urls.txt 

cat xss_urls.txt | sed 's/=.*/=/' > loxs.txt

python3 loxs.py (fed the loxs.txt with xss payloads available there)
```
{% endcode %}
