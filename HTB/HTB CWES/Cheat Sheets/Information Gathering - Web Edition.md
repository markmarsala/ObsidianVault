**Subdomain Brute-Forcing
```
dnsenum example.com -f subdomains.txt
```

**Zone Transfers
```
dig @ns1.example.com example.com axfr
```

**Vhost Brute-Forcing
```
gobuster vhost -u http://192.0.2.1 -w hostnames.txt
```

**Certificate Transparency (CT) Logs
```
curl -s "https://crt.sh/?q=%25.example.com&output=json" | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u
```

**Web Crawling
```
import scrapy

class ExampleSpider(scrapy.Spider):
    name = "example"
    start_urls = ['http://example.com/']

    def parse(self, response):
        for link in response.css('a::attr(href)').getall():
            if any(link.endswith(ext) for ext in self.interesting_extensions):
                yield {"file": link}
            elif not link.startswith("#") and not link.startswith("mailto:"):
                yield response.follow(link, callback=self.parse)
```
```
jq -r '.[] | select(.file != null) | .file' example_data.json | sort -u
```
- Check robots.txt
