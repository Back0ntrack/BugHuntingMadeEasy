# Phase - 1: Recon

**Recon techniques are broadly classified into Wide Recon and Narrow Recon.**

## Wide Recon

Wide recon identifies applications, subdomains, and assets, expanding the scope of work and increasing the chances of discovering unknown assets. This makes it highly useful for Red Team engagements.

What are we looking for ?&#x20;

* APEX Domains
* Subdomains
* IP Addresses
* Services running on the target

## Narrow Recon

Narrow recon focuses on digging deeper into the assets discovered during wide recon. The goal is to enumerate directories, endpoints, and specific technologies in use. This stage helps identify attack surfaces by narrowing the scope to only **live, reachable, and relevant** targets. It plays a critical role in mapping out how the target functions internally.

**What are we looking for?**

* Alive Subdomains
* Directory and Endpoint Fuzzing (`/admin`, `/login`, `/api`, etc.)
* Web Page Discovery and Crawling
* Fingerprinting Technologies (CMS, frameworks, WAFs)
* GitHub Dorking and Code Leak Detection
* Javascript File Analysis (to extract hidden endpoints and API calls)
* Parameter Discovery (`?id=`, `?q=`, etc.)
* Robots.txt, Sitemap.xml, and Hidden Files
