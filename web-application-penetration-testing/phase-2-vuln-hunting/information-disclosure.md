# Information Disclosure

## What it is ?&#x20;

Information disclosure, also known as information leakage, is when a website unintentionally reveals sensitive information to its users. Depending on the context, websites may leak all kinds of information, including:&#x20;

* Data about other users, such as usernames or financial information
* Sensitive commercial or business data
* Technical details about websites and its infrastructure

## What are we looking for ?&#x20;

* Directory Listings
* Developer Comments
* Error Messages/Stack Trace
* Sensitive/Source code disclosures via backup files
* JavaScript Files (API Endpoints, Keys)
* Default credentials/Exposed Login Portals

***

## How to find ?&#x20;

* _Arbitrary input to parameters_
* _Checking source code for comments_
* _Checking known directories and try to bypass it with headers \[Make sure you've checked all available HTTP methods]_
* _Fuzzing \[Make sure to create AI powered directory list, page list, parameter and value list]_
* _Crawling JS files_
