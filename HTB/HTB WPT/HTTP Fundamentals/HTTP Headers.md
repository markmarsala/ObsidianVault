1. General Headers
2. Entity Headers
3. Request Headers
4. Response Headers
5. Security Headers

**General Headers
- Date
- Connection

**Entity Headers
- Content-Type
- Media-Type
- Boundary
- Content-Length
- Content-Encoding

**Request Headers
- Host
- User-Agent
- Referer
- Accept
- Cookie
- Authorization

**Response Headers
- Server
- Set-Cookie
- WWW-Authenticate

**Security Headers
- Content-Security-Policy
- Strict-Transport-Security
- Referrer-Policy

```
curl -I https://www.inlanefreight.com
curl https://www.inlanefreight.com -A 'Mozilla/5.0'
```
- I only displays headers
- A sets user agent
