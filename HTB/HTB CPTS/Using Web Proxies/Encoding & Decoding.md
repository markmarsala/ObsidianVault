Encoding / Decoding

## Encoding - Decoder tab

Key characters we need to encode:
- Spaces: may indicate the end of request data if not encoded
- &: otherwise interpreted as a paameter delimiter
- #: otherwise interpreted as a fragment delimiter

## URL-encode text in Burp Repeater
- Convert Selection > URL > > URL encode key characters OR
- Select text and click [CTRL+U] OR
- Right click and enable URL-encoding

ZAP automatically URL-encodes all of our request data in the background before sending the request

Encoders:
- HTML
- Unicode
- Base64
- ASCII hex

## Decoders
ZAP -> [CTRL+E]
Burp -> Inspector too to peform encoding and decoding
