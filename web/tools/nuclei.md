---
cover: ../../.gitbook/assets/all_in_one_cover (2).PNG
coverY: 0
---

# Nuclei

### Scanning a single target

```
nuclei -u http://example.com
```

OR

```
nuclei -target http://example.com
```

<figure><img src="../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

### Scanning targets from a file

```
nuclei -l targets.txt
```

### Using Template Folders (Using Specific templates by files and folders)

```
# Using Specific Template by folder
nuclei -t http/exposures/

# Using Specific Template by file
nuclei -u https://my.target.site -t exposures/files/pyproject-disclosure.yaml
```

### Updating Templates&#x20;

Update nuclei installation to latest versions:

```
nuclei -up
```

Update templates:&#x20;

```
nuclei -ut
```



