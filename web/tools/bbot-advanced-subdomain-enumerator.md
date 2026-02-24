# bbot \[Advanced Subdomain Enumerator]

## Installation

<div data-full-width="true"><figure><img src="../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure></div>

### Adding API keys

* Open the `/root/.config/bbot/secret.yml` file.&#x20;
* Locate your desired service (e.g., `securitytrails`, `shodan_dns`, `virustotal`, etc.) under the `modules:` section.&#x20;
* Uncomment only that block, maintaining correct indentation (use 2 spaces, no tabs).&#x20;
* Add your api keys in this format:&#x20;

{% code lineNumbers="true" %}
```
modules:
  securitytrails:
    api_key: your_api_key_here
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

> **Note: YAML** is space sensitive. Indentation must be consistent (use 2 spaces per level).&#x20;

{% code overflow="wrap" %}
```
Future Prompt for ChatGPT: 
Uncomment only the required module <add required modules herein> in the secrets.yml file for BBOT with api_key: bighunter and keep everything else commented.
```
{% endcode %}

## Usage

### Presets \[bbot capabilities]

List Presets: `bbot -lp`

Some good presets: `subdomain-enum`, `cloud-enum`, `code-enum`

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

The only difference between the setting up flags and presets is when we set preset then all the modules is being used but when we use flags then only required modules to that flag is used.

> BBOT is a powerful OSINT tool. Even without API keys, it performs extensive subdomain discovery using multiple passive methods and recursive enrichment. It uses a plugin chain system that feeds results between modules to dig deeper, such as resolving found domains or generating permutations. You can view which tools and methods are used in each scan type by running `bbot list-presets`.

### Output Modules

Standard output modules: `csv`, `json`, `python`, `stdout`, `txt`, `subdomains`

Check available Output Modules: `bbot -lo`

### All at once

```
bbot -t <target.com> -p kitchen-sink
```

### Subdomain Discovery

Preset Used: `subdomain-enum`

{% code overflow="wrap" %}
```bash
bbot -t <target.com> -p subdomain-enum -m <module to use> -om txt,csv,subdomains -n aequs_subs -o ~/Projects/aequs/

E.g., ./bbot -t evil.com -p subdomain-enum -m securitytrails,bevigil
```
{% endcode %}

{% hint style="info" %}
The `subdomains` output module will list only subdomains from the output.&#x20;
{% endhint %}

{% code overflow="wrap" %}
```
Available API keys: 
shodan, SecurityTrails, censys.io, binaryedge, bevigil, virustotal, projectdiscovery, hunter
```
{% endcode %}

### Best Usage

{% code overflow="wrap" %}
```bash
bbot -t <target.com> -s -o ./myDir -n targetScan -p kitchen-sink -om asset_inventory,csv,txt,subdomains,stdout
```
{% endcode %}

{% code overflow="wrap" %}
```
bbot -t <target.com> -s -o ./myDir -n targetScan -p subdomain-enum,cloud-enum,nuclei,email-enum -om asset_inventory,csv,txt,subdomains,stdout,emails --allow-deadly
```
{% endcode %}
