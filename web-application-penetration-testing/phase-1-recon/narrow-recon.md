# Narrow Recon

<figure><img src="../../.gitbook/assets/Narrow Recon (1).png" alt=""><figcaption><p><a href="https://xmind.ai/share/SIXIEVXc">https://xmind.ai/share/SIXIEVXc</a></p></figcaption></figure>

## Alive Subdomain check

```
cat subdomains.txt | httpx -mc 200 -o alive_subdomains.txt
cat subdomains.txt | httpx -mc 302,301 -o redirecting_subdomains.txt
```

## Fuzzing&#x20;

**Fuzzing in web applications** is a dynamic testing technique that involves automatically sending large volumes of unexpected, invalid, or random data inputs to web application endpoints to identify security vulnerabilities, crashes, or unexpected behavior.

### Some good wordlist for fuzzing&#x20;

{% code lineNumbers="true" %}
```bash
** Allinone **
https://github.com/danielmiessler/SecLists
https://github.com/orwagodfather/WordList
https://gist.github.com/jhaddix/86a06c5dc309d08580a018c66354a056
https://github.com/1BlackLine/Payloads
https://github.com/Karanxa/Bug-Bounty-Wordlists
https://github.com/HacktivistRO/Bug-Bounty-Wordlists/tree/master/Wordlists

** Jason Haddix: Context Discovery Special **
https://gist.github.com/jhaddix/b80ea67d85c13206125806f0828f4d10

** Database Specific File **
https://github.com/dkcyberz/Harpy/blob/main/Hidden/database.txt
```
{% endcode %}

### Directory Fuzzing

```bash
ffuf -w <wordlist> -u https://target.com/FUZZ -ic -t 200 -c
```

### Extension Fuzzing

**Check on which utility the site is running.** The extensions must contain `.`

```bash
ffuf -w <wordlist> -u http://target.com/blog/indexFUZZ -ic -t 200 -c
```

### Page Fuzzing

```bash
ffuf -w <wordlist> -u https://target.com/blog/FUZZ.php -ic -c -t 200
```

If you find any directory such as `logs` or `backup` make sure that you've created your own wordlist from chatgpt. Generally log folder can include file of this types: `log-<date>.log` or `log<date>` etc. Be practical. Work according to the scenarios.&#x20;

### Check for backup and Old Files

```bash
ffuf -u https://target.com/FUZZ -w wordlist.txt -x .bak,.old,.zip,.tar.gz
```

### Recursive Fuzzing

Above we first fuzz for directories and then fuzzing for pages under these directories. But using recursive fuzzing we can fuzz for both.

In recursive fuzzing if we specify the `-recursion-depth 1`, it will only fuzz the main directories and their direct sub-directories. We can specify extension with `-e .php`.

{% code overflow="wrap" %}
```bash
ffuf -w <wordlist> -u https://target.com/FUZZ -recursion -recursion-depth 1 -e .php -v -ic -c
```
{% endcode %}

### Subdomain Fuzzing

```bash
ffuf -w <wordlist> -u https://FUZZ.target.com/ -ic -c -t 200
```

### Vhosts fuzzing

```bash
ffuf -w <wordlist> -u https://target.com/ -H 'Host: FUZZ.target.com' -ic -c -t 200
```

### Parameter and Value Fuzzing&#x20;

#### GET Request

{% code overflow="wrap" %}
```bash
** Parameter fuzzing **
ffuf -w seclists/Discovery/Web-Content/burp-parameter-names.txt -u https://target.com/admin/admin.php?FUZZ=key -fs 900 -ic -c

** Value Fuzzing **
for i in $(seq 1 1000); do echo $i >> ids.txt; done
ffuf -w ./ids.txt -u https://target.com/admin/admin.php?id=FUZZ -fs 900 -ic -c
```
{% endcode %}

#### POST Request

{% code overflow="wrap" %}
```bash
** Parameter Fuzzing **
ffuf -w <wordlist> -u https://target.com/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/X-www-form-urlencoded' -fs 900

** Value Fuzzing **
ffuf -w ./ids.txt -u https://target.com/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/X-www-form-urlencoded' -fs 900
```
{% endcode %}

#### Fuzzing both parameter and value

{% code overflow="wrap" %}
```bash
** POST **
ffuf -w param_list.txt:PARAM -w value_list.txt:VAL -u https://target.com/admin/admin.php -X POST -d 'PARAM=VAL' -H 'Content-Type: application/X-www-form-urlencoded' -fs 900

** GET **
ffuf -w params.txt:PARAM -w values.txt:VAL -u https://target.com/?PARAM=VAL -mr "VAL" -c
#Fuzz multiple locations. Match only responses reflecting the value of "VAL" keyword. Colored
```
{% endcode %}

## Hidden Dev files

When you find a .git folder on a web server, you've struck gold. Here's why:

* `.git/HEAD`: Points to the current branch (e.g., "ref: refs/heads/master")
* `.git/config`: Contains repository config including remote URLs and credentials
* `.git/index`: Database of all files in the repository
* `.git/objects/`: Contains all versions of every file (compressed)
* `.git/refs/`: Contains commit hashes for branches and tags

**Why It's Dangerous:** If a .git folder is exposed, attackers can reconstruct the entire source code by:

1. Downloading the .git folder
2. Using the index file to identify all objects
3. Downloading each object from .git/objects/
4. Rebuilding files from these objects Tools like git-dumper automate this process entirely. `gitextractor` is a tool that will help extract sensitive contents from the `.git` folder.&#x20;

### Other Common Exposed Files Examples and Their Purpose

1. **Container Files**
   * `docker-compose.yml`: Multi-container setup with passwords and volumes
   * `Dockerfile`: Shows how the app is built and what's installed
   * `kubernetes.yaml`: Contains service accounts and secrets
2. **Config Files**
   * `.env`: Environment variables with database passwords
   * `wp-config.php`: WordPress database credentials
   * `config.php`: Application secrets and API keys
3. **Build Files**
   * `package.json`: Shows dependencies and scripts
   * `.npmrc`: Can contain private registry tokens
   * `Jenkinsfile`: Shows deployment processes
4. **IDE Files**
   * .vscode/: Contains debugging configurations
   * .idea/: Can expose local file paths
   * .swp: Vim temporary files with unsaved changes

Each of these files can expose sensitive details about your application's infrastructure, credentials, or internal workings.

## DNS Zone Transfer

### Using dnsrecon

```
dnsrecon -d <domain.com> -t axfr 

   === OR ===
   
dnsrecon -d <domain.com> -a
```

### Using dig

```
dig axfr inlanefreight.com @ns.inlanefreight.com
```

### Using nslookup

```
C:\Users\Administrator> nslookup
> set query=SOA
> inlanefreight.com
> ls -d inlanefreight.com
```

## Fingerprinting

Fingerprinting focuses on extracting technical details about the technologies powering a website or web application.&#x20;

* [Wappalyzer extension](https://chromewebstore.google.com/detail/wappalyzer-technology-pro/gppongmhjkpfnbhagpmjfkannfbllamg?hl=en)
* [BuiltWith](https://builtwith.com/)
* WhatWeb tool

## GitHub Dorking

GitHub dorking is the process of searching for exposed secrets and misconfigured repositories using GitHub's search functionality.&#x20;

### Automation:&#x20;

* [https://gist.github.com/jhaddix/1fb7ab2409ab579178d2a79959909b33](https://gist.github.com/jhaddix/1fb7ab2409ab579178d2a79959909b33)
* [TruffleHog](https://github.com/trufflesecurity/truffleHog)
  * `trufflehog git <repo_link> --only-verified`&#x20;
  * Browser extension: [https://addons.mozilla.org/en-US/firefox/addon/trufflehog/](https://addons.mozilla.org/en-US/firefox/addon/trufflehog/)
* [Gitrob](https://github.com/michenriksen/gitrob)

### GitHub's Search Syntax

Where to search: [https://gist.github.com/search](https://gist.github.com/search)

* `"nasa.gov"`: search for a specific domain. Then use the below keywords.&#x20;
* `filename`: Searches for specific filenames. E.g., `filename:.env` search for .env files
* `extension`: Filters search results by file extension. E.g., `extension:sql`
* `path`: searches for file in specific directories. `path:config`
* `in`: Searches for a term in speciifc parts of the repository. `in:readme`
* `repo`: Searches within a specific repository. `repo:username/repositoryname "API_KEY"`
* `org`: Searches within a specific organization. `org:microsoft "client_secret"`
* `language`: Filter results by programming language. `Language: python "AWS_SECRET_ACCESS_KEY"`



### Common GitHub Dorking Queries

#### Finding API key and Credentials

* `filename:.env DB_PASSWORD`
* `filename:.env AWS_ACCESS_KEY_ID`
* `filename:config.json auth`
* `filename:settings.py SECRET_KEY`

### Finding Harcoded Credentials

* `password extension:ini`
* `password extension:sql`
* `apikey extension:json`

### Finding Private SSH keys

* `filename:id_rsa private`
* `filename:.ssh`

### Finding Database credentials

* `filename:wp-config.php`
* `filename:.htpasswd`

**Best Usage:**&#x20;

{% code overflow="wrap" %}
```bash
filename:config OR filename:.env OR filename:.pem OR filename:.key OR filename:database OR filename:password
```
{% endcode %}

## Crawling and Bug

Web crawling, in the context of bug bounty refers to the process of automatically navigating through a website’s structure to discover various resources such as web pages, files and scripts. This is done by following links and examining directories to create a map of a website’s content.



What are we after ?&#x20;

* Discover Hidden or Less Obvious Endpoints: `/admin/`, `/api/v1/token`, `/backup.zip`
* Uncover Parameterized URLs: `/search.php?q=term`, `/user?id=1001`
  * useful for testing SQLi, XSS, LFI, IDOR
* Find sensitive files



### Crawling API Endpoints

{% code overflow="wrap" %}
```bash
katana -u https://www.example.com -d 5 -mdc 'contains(endpoint,"api")' -jc -jsl -kf all -o api_endpoints.txt
```
{% endcode %}

```bash
ffuf -u http://api.target.com/FUZZ -w wordlist/api_endpoints.txt -c -ic -t 50
```

### Crawling JS Files and find sensitive data

#### Fetching all urls

{% code overflow="wrap" %}
```bash
#Collecting URLs for Open Redirect
#Method 1
 echo 'simplilearn.com' | gau | grep '^https://[^/]*\.simplilearn\.com' > gau_urls.txt

#Method 2
echo 'https://www.simplilearn.com' | hakrawler -d 3 --subs -t 30 | grep '^https://[^/]*\.simplilearn\.com' > hakrawler_urls.txt

#Method 3
 katana -list simplilearn/recon/sub_enum/combined.txt -d 5 -jc -jsl -c 30 | grep '^https://[^/]*\.simplilearn\.com' > katana_urls.txt
 
 #Method 4
 waybackurls 'simplilearn.com' | grep '^https://[^/]*\.simplilearn\.com' > wayback_urls.txt
 
 #Method 5
 paramspider -d 'simplilearn.com' -p -s 
```
{% endcode %}

```bash
cat js_files.txt | uro | sort -u | httpx-toolkit -mc 200 -t 150 -o target_js_urls.txt
```

{% code overflow="wrap" %}
```
Using the endpointer and lazyegg extension of chrome which will get all the endpoints. In the panel section we can see various endpoints and whether iff it is vulnerable. For more information check JS Recon Masterclass Lostsec. 
```
{% endcode %}

#### Finding sensitive info

{% code overflow="wrap" %}
```bash
cat allurls.txt | grep -E "\.js$" | httpx-toolkit -mc 200 -content-type | grep -E "application/javascript|text/javascript" | cut -d' ' -f1 | xargs -I% curl -s % | grep -E "(API_KEY|api_key|apikey|secret|token|password)"
```
{% endcode %}

```bash
cat js-urls.txt | jsleak -s -k
# -s will run secretfinder
# -l will run linkfinder (disable it because it would find link only.)
# -k check status
```

```bash
nuclei -l js.txt -t ~/nuclei-templates/exposures

cat js.txt | nuclei -t credentials-disclosure-all.yaml -c 30
#template available in https://github.com/coffinxp/nuclei-templates
```

{% code overflow="wrap" %}
```bash
cat js.txt | xargs -I{} bash -c 'echo -e "\ntarget : {}\n" && python lazyegg.py "{}" --js_urls --domains --ips --leaked_creds --local_storage'
```
{% endcode %}

{% code overflow="wrap" %}
```bash
cat target_js_urls.txt | while read url; do python3 SecretFinder.py -i $url -o cli; done
```
{% endcode %}

{% code overflow="wrap" %}
```bash
cat target_js_urls.txt | while read url; do 
  echo "[+] Checking: $url"
  curl -s "$url" | grep -Eoi "aws_access_key|aws_secret_key|api[-_ ]?key|passwd|pwd|heroku|slack|firebase|swagger|aws[-_ ]?key|password|ftp password|jdbc|db|sql|secret|config|admin|json|gcp|htaccess|.env|ssh[-_ ]?key|.git|access[-_ ]?key|token|oauth_token|oauth_token_secret" && echo "↑ Found in: $url"
done
```
{% endcode %}

#### Finding hidden endpoints from JS Files

```bash
python3 linkfinder.py -i js.txt -o endpoints.txt
```

### Crawl juicy endpoints

{% code overflow="wrap" %}
```bash
katana -u https://target.com -d 5 -jc -jsl -o all_urls.txt
cat "https://target.com" | hakrawler -d 5 -subs | grep -E '*target*' >> all_urls.txt

grep -Ei '/(admin|config|dashboard|login|auth|debug|test|staging|upload|backup|server-status|monitor|manage|dev|portal|private|panel|root|internal|console|cgi-bin|shell|setup|editor|password|credentials|db|database|env|hidden|system|account|superuser|core|includes|wp-admin|webadmin|cpanel|git|svn|api|v1|v2|token|key|secret|jwt|session|logs|error|secure|restricted|flag)(/|$)' all_urls.txt | sort -u > juicy_urls.txt
```
{% endcode %}

### Finding Juicy information

* `kakhunt` tool which demands url and you're good to go. GUI based.&#x20;

## Find Sensitive parameters

Tools such as ParamSpider and Arjun can discover hidden or less obvious parameters, which are sometimes difficult to find, but can be advantageous for identifying vulnerabilities like IDOR, XSS, SQL injection, and more.

{% code overflow="wrap" %}
```bash
paramspider -d <domain> [automatically save results]
arjun -u <domain> -oT params.txt 

grep -Ei 'url=|uri=|redirect=|next=|data=|dest=|callback=|return=|page=|path=|continue=|token=|secret=|debug=|file=|include=|document=|template=|php_path=|req=|cmd=|exec=|query=|search=|q=|s=|id=|user=|admin=|password=|email=' params.txt
```
{% endcode %}

## JS Enumeration and Fetching URLs

{% code title="js_recon_and_analysis_commands.sh" overflow="wrap" %}
```bash
#!/bin/bash

# ANSI Colors
RED="\e[31m"
GREEN="\e[32m"
YELLOW="\e[33m"
BLUE="\e[34m"
MAGENTA="\e[35m"
CYAN="\e[36m"
BOLD="\e[1m"
NC="\e[0m"

# Get domain input
read -p $'\e[33;1mEnter target domain (e.g., simplilearn.com): \e[0m' DOMAIN
BASE=$(echo "$DOMAIN" | cut -d '.' -f1)

# Create required folder structure
mkdir -p "${BASE}/recon/js_enum/dump"

# Check for combined.txt in sub_enum
COMBINED_PATH="${BASE}/recon/sub_enum/combined.txt"
if [[ ! -f "$COMBINED_PATH" ]]; then
  echo -e "${RED}❌ ${COMBINED_PATH} not found.${NC}"
  read -p $'\e[33;1mEnter full path to domains.txt (combined subdomains): \e[0m' COMBINED_PATH
  if [[ ! -f "$COMBINED_PATH" ]]; then
    echo -e "${RED}❌ Still not found. Exiting.${NC}"
    exit 1
  fi
fi

# Output file
COMMANDS_FILE="js_commands.txt"

{
  toilet -f digital 'JS Recon & Analysis' | lolcat --force -p 1

  toilet -F border -f term "URL Collection" | lolcat --force -p 0.2

  echo -e "${CYAN}▶ echo 'https://www.${DOMAIN}' | hakrawler -d 3 --subs -t 30 | grep '^https://[^/]*\\.${BASE}\\.com' > ${BASE}/recon/js_enum/dump/hakrawler_urls.txt${NC}"
  echo -e "${MAGENTA}────────────────────────────────────────────────────────────────────${NC}"
  echo -e "${CYAN}▶ katana -list ${COMBINED_PATH} -d 5 -jc -jsl -c 30 | grep '^https://[^/]*\\.${BASE}\\.com' > ${BASE}/recon/js_enum/dump/katana_urls.txt${NC}"
  echo -e "${MAGENTA}────────────────────────────────────────────────────────────────────${NC}"
  echo -e "${CYAN}▶ waybackurls '${DOMAIN}' | grep '^https://[^/]*\\.${BASE}\\.com' > ${BASE}/recon/js_enum/dump/wayback_urls.txt${NC}"

  toilet -F border -f term "Filter JS & Alive URLs" | lolcat --force -p 0.2

  echo -e "${CYAN}▶ cat ${BASE}/recon/js_enum/dump/wayback_urls.txt ${BASE}/recon/js_enum/dump/hakrawler_urls.txt ${BASE}/recon/js_enum/dump/katana_urls.txt | anew ${BASE}/recon/js_enum/dump/all_urls.txt"
  echo -e "${MAGENTA}────────────────────────────────────────────────────────────────────${NC}"
  echo -e "${CYAN}▶ cat ${BASE}/recon/js_enum/dump/all_urls.txt | grep '\\.js\$' | uro | sort -u | httpx-toolkit -mc 200 -t 150 -o ${BASE}/recon/js_enum/target_js_urls.txt${NC}"
  echo -e "${MAGENTA}────────────────────────────────────────────────────────────────────${NC}"
  echo -e "${CYAN}▶ cat ${BASE}/recon/js_enum/dump/all_urls.txt | uro | sort -u | httpx-toolkit -mc 200,302 -t 150 -o ${BASE}/recon/js_enum/target_url_list.txt${NC}"

  toilet -F border -f term "Sensitive Info & Leaks" | lolcat --force -p 0.2

  echo -e "${CYAN}▶ cat ${BASE}/recon/js_enum/target_js_urls.txt | jsleak -s -k${NC}"
  echo -e "${MAGENTA}────────────────────────────────────────────────────────────────────${NC}"
  echo -e "${CYAN}▶ nuclei -l ${BASE}/recon/js_enum/target_js_urls.txt -t ~/.local/nuclei-templates/http/exposures -c 30${NC}"
  echo -e "${MAGENTA}────────────────────────────────────────────────────────────────────${NC}"
  echo -e "${CYAN}▶ cat ${BASE}/recon/js_enum/target_js_urls.txt | nuclei -t ~/.local/nuclei-templates/coffinxp_nuclei_templates/credentials-disclosure-all.yaml -c 30${NC}"

  toilet -F border -f term "Optional Extra Tools" | lolcat --force -p 0.2

  echo -e "${CYAN}▶ echo '${DOMAIN}' | gau | grep '^https://[^/]*\\.${BASE}\\.com' | anew ${BASE}/recon/js_enum/dump/urls.txt${NC}"
  echo -e "${CYAN}▶ cat ${BASE}/recon/js_enum/dump/urls.txt | anew ${BASE}/recon/js_enum/target_url_list.txt${NC}"
  echo -e "${YELLOW}💡 If any new JS URLs are found, restart the JS analysis phase for them.${NC}"

  echo -e "${MAGENTA}────────────────────────────────────────────────────────────────────${NC}"
  echo -e "${CYAN}▶ cat ${BASE}/recon/js_enum/target_js_urls.txt | xargs -I{} bash -c 'echo -e \"\\ntarget : {}\\n\" && lazyegg \"{}\" --js_urls --domains --ips --leaked_creds --local_storage'${NC}"
  echo -e "${YELLOW}⚠️ This command is powerful but messy. Use only when ready for deep analysis.${NC}"

  echo -e "\n${GREEN}${BOLD}✅ JS Recon & Analysis completed.${NC}"
  echo -e "${YELLOW}You now have filtered URLs and JS files. You can start hunting or analyze leaks manually.${NC}"

    toilet -F border -f term "Crawling Juicy Endpoints" | lolcat --force -p 0.2

  echo -e "${CYAN}▶ grep -Ei '/(admin|config|dashboard|login|auth|debug|test|staging|upload|backup|server-status|monitor|manage|dev|portal|private|panel|root|internal|console|cgi-bin|shell|setup|editor|password|credentials|db|database|env|hidden|system|account|superuser|core|includes|wp-admin|webadmin|cpanel|git|svn|api|v1|v2|token|key|secret|jwt|session|logs|error|secure|restricted|flag)(/|$)' ${BASE}/recon/js_enum/target_url_list.txt | sort -u > ${BASE}/recon/js_enum/juicy_urls.txt${NC}"

  echo -e "${MAGENTA}────────────────────────────────────────────────────────────────────${NC}"

  echo -e "${CYAN}▶ katana -u https://www.${DOMAIN} -d 5 -mdc 'contains(endpoint,\"api\")' -jc -jsl -kf all -o ${BASE}/recon/js_enum/dump/api_endpoints.txt${NC}"

  echo -e "${MAGENTA}────────────────────────────────────────────────────────────────────${NC}"

  echo -e "${CYAN}▶ cat ${BASE}/recon/js_enum/dump/api_endpoints.txt | uro | sort -u | httpx-toolkit -mc 200 -t 150 -o ${BASE}/recon/js_enum/juicy_api_endpoints.txt${NC}"


} | tee "$COMMANDS_FILE"
```
{% endcode %}
