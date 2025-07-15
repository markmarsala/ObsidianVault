
Burp Suite -> Proxy > Intercept > Open Browser
ZAP -> Firefox symbol

To select different port:

Burp Suite -> Proxy > Options
ZAP -> Tools > Options > Local Proxies


To start proxy on Firefox:
- Firefox preferences OR
- Firefox Extensions Page > Add to Firefox > Foxy Proxy


**Foxy Proxy**
Options > add > 127.0.0.1 for IP > 8080 for port > name Burp or ZAP > click Foxy Proxy icon > Burp/ZAP


**Installing CA Certificate**
Burp -> Browse to http://burp > CA Certificate > Download
ZAP -> Tools > Options > Dynamic SSL Certificate > Generate > Save

**Installing CA Certificate on Firefox**
Browse to about:preferences#privacy > View Certificates > Authorities tab > Import > select downloaded CA certificate > Trust this CA to identify websites > Trust this CA to identify email users > OK

