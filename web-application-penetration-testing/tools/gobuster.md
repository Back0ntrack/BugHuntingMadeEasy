# Gobuster

### DNS Mode

**Subdomain Enumeration**

```bash
gobuster dns -d mysite.com -t 50 -w common_names.txt

Options: 
-i : Show IP
```

### DIR Mode

**Directory bruteforce**

```bash
gobuster dir -u <https://mysite.com> -t 50 -w common_files.txt
```

### VHOST mode

```bash
gobuster vhost -u <https://mysite.com> -w common_Vhosts.txt --append-domain
```

### Fuzz mode

```bash
gobsuter fuzz -u <https://example.com?FUZZ=test> -w parameter-names.txt
```
