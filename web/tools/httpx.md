# HTTPX

`httpx` is a fast and multi-purpose HTTP toolkit that allows running multiple probes using the retryablehttp library. It is similar to curl but fast and have multiple filters available.&#x20;

### Input Options

* `-l`: input file containing list of hosts to process
* `-rr`: file containing raw request string&#x20;
* `-u`: input target host to probe.&#x20;



### Probes

* `-sc`: display response status-code. \[Not Useful]
* `-location`: display response redirect location.



### Matchers \[response with specific matchers]

* `-mc`: match response with specifc status code (`-mc 200,302`)
* `-ml`: Match response with specified content length (`-ml 100,102`)



### Filters \[response excluding specific matchers]

* `-fc`: filter response with specfied status code (`-fc 403,401`)



`-t`: specify threads



### Best Usage

```
cat subdomains.txt | httpx -mc 200 -o alive_subdomains.txt
cat subdomains.txt | httpx -mc 302,301 -o redirecting_subdomains.txt
```



