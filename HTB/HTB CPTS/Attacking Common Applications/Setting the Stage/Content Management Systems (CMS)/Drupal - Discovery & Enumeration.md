## Discovery/Footprinting
```
curl -s http://drupal.inlanefreight.local | grep Drupal
```
- Another way to identify Drupal is through nodes. The URIs are usually of the form /node/<nodeid>

## Enumeration
```
curl -s http://drupal-acc.inlanefreight.local/CHANGELOG.txt | grep -m2 ""
curl -s http://drupal.inlanefreight.local/CHANGELOG.txt
droopescan scan drupal -u http://drupal.inlanefreight.local
```

Drupal vulnerabilities: https://www.cvedetails.com/vulnerability-list/vendor_id-1367/product_id-2387/Drupal-Drupal.html
