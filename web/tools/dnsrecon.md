# dnsrecon

## Base Domain Enumeration (All)

```bash
dnsrecon -d <domain.com>
```

### Brute forcing Domain

```bash
dnsrecon -d <domain.com> -D <path to dictionary file> -t brt
```

### DNS Zone Transfer

```bash
dnsrecon -d <domain.com> -t axfr 

   === OR ===
dnsrecon -d <domain.com> -a
```

### Reverse Lookup

```bash
 dnsrecon -r 208.67.222.200-208.67.222.255 -d microsoft.com
```
