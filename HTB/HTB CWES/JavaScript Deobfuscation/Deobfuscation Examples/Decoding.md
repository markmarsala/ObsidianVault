- Base64
- Hex
- Rot13

**Base64
```
echo https://www.hackthebox.eu/ | base64
echo aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K | base64 -d
```
- d for decode


**Hex
```
echo https://www.hackthebox.eu/ | xxd -p
echo aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K | xxd -p -r
```
- p r for decode


**Hex
```
echo https://www.hackthebox.eu/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
echo uggcf://jjj.unpxgurobk.rh/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```


Cipher Identifier: https://www.boxentriq.com/code-breaking/cipher-identifier
