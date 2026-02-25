Both jq and curl are available in Ubuntu 24.04's default repos. If not already installed:

```bash
sudo apt-get install -y jq curl
```

**Command**

```bash
jq -r 'select(.symbol == "TSLA" and .side == "sell") | .order_id' ./transaction-log.txt | xargs -I {} echo "https://example.com/api/{}" > ./output.txt 
```

Example output:
```
https://example.com/api/12346
https://example.com/api/12362
```

1. *`jq -r 'select(...) | .order_id'`*  Reads each JSON line, filters for TSLA sells, and outputs the raw (-r) order_id — one per line (12346, 12362).
2. *`xargs -I {} curl -s "https://example.com/api/{}"`* For each order_id piped in, substitutes {} into the URL and executes a silent (-s) HTTP GET request via curl.
3. *`> ./output.txt`* Redirects all combined curl output into ./output.txt (use >> to append if the file already has content you want to keep).
