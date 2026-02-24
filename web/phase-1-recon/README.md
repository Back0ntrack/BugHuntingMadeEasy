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

## Some Important questions while doing recon

### Q1. Are you limited to the apex domain or are you targeting the entire company?

If the scope is limited to the apex domain (e.g., `example.com`), focus only on discovering and enumerating subdomains under that domain. Avoid ASN-based reconnaissance, IP-range sweeps, acquisition mapping, or infrastructure correlation beyond that apex. Those steps are valuable only when the entire company or organization is in scope.

#### **Q2. Should I wait for all recon tools to complete before starting testing?**

No, you don’t need to wait for all recon tools to finish. Run a fast, accurate subdomain enumerator (like `amass enum -passive`, `subfinder`, or `assetfinder`) to get initial targets quickly. Begin testing them while deeper scans (like full Amass active enumeration, brute-force, and DNS resolvers) run in parallel. This balances time and coverage efficiently.

#### **Q3. Do I need to use passive, active, or hybrid recon?**

**Answer:**

* **Passive recon** is safe for initial discovery and avoids detection (e.g., `crt.sh`, `shodan`, `securitytrails`, `subfinder`).
* **Active recon** (e.g., DNS brute-force, `nmap`, `masscan`) should only be used if allowed in scope.
* In bug bounties, prefer **passive-first → hybrid → active (if permitted)**.

#### **Q9. When to stop recon?**

Recon isn’t something you “finish” — it’s iterative. Stop a scan temporarily when diminishing returns appear (few new domains found) or when you have enough actionable assets to test.
