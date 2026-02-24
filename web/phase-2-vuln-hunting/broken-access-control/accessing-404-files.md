# Accessing 404 files

## Accessing 404 Files

#### Search sensitive files in the below results (such as `.pdf`, `.csv`, `.backup`, `.rar`) and enter its url in <kbd>wayback archive</kbd>:&#x20;

* `https://web.archive.org/cdx/search/cdx?url=*.simplilearn.com/*&collapse=urlkey&output=text&fl=original`
* `https://www.virustotal.com/vtapi/v2/domain/reort?apikey=<apikey>&domain=example.com`
* `https://otx.alienvault.com/api/v1/indicators/hostname/domain.com/url_list?limit=500&page=1`

#### Using bash for the same

```bash
#!/bin/bash

# Prompt for the domain name.
read -p "Enter the domain (e.g., example.com): " domain

echo "Fetching all URLs from the Wayback Machine..."
# Fetch all URLs for the given domain.
curl -G "https://web.archive.org/cdx/search/cdx" \
  --data-urlencode "url=*.$domain/*" \
  --data-urlencode "collapse=urlkey" \
  --data-urlencode "output=text" \
  --data-urlencode "fl=original" \
  -o all_urls.txt

echo "Fetching URLs with specific file extensions..."
# Fetch only URLs ending with certain file extensions.
# (Adjust the extension list if necessary.)
curl "https://web.archive.org/cdx/search/cdx?url=*.$domain/*&collapse=urlkey&output=text&fl=original&filter=original:.*\.(xls|xml|xlsx|json|pdf|sql|doc|docx|pptx|txt|git|zip|tar\.gz|tgz|bak|7z|rar|log|cache|secret|db|backup|yml|gz|config|csv|yaml|md|md5|exe|dll|bin|ini|bat|sh|tar|deb|rpm|iso|img|env|apk|msi|dmg|tmp|crt|pem|key|pub|asc)$" \
  -o filtered_urls.txt

echo "Done! Results saved to:"
echo "  - all_urls.txt (all URLs)"
echo "  - filtered_urls.txt (URLs with specific file extensions)"
```

{% code overflow="wrap" %}
```bash
cat all_urls.txt | uro | grep -E '\.xls|\.xml|\.xlsx|\.json|\.pdf|\.sql|\.doc|\.docx|\.pptx|\.txt|\.zip|\.tar\.gz|\.tgz|\.bak|\.7z|\.rar|\.log|\.cache|\.secret|\.db|\.backup|\.yml|\.gz|\.config|\.csv|\.yaml|\.md|\.md5|\.exe|\.dll|\.bin|\.ini|\.bat|\.sh|\.tar|\.deb|\.rpm|\.iso|\.img|\.apk|\.msi|\.dmg|\.tmp|\.crt|\.pem|\.key|\.pub|\.asc'
```
{% endcode %}
