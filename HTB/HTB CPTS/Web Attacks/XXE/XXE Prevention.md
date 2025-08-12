**Avoiding Outdated Components

- Update XML libraries
- Update components that parse XML input, such as API libraries like SOAP
- Update file processors like SVG image processors or PDF document processors
https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html#php

**Using Safe XML Configurations
- Disable referencing custom Document Type Definitions (DTDs)
- Disable referencing External XML Entities
- Disable Parameter Entity processing
- Disable support for XInclude
- Prevent Entity Reference Loops

**Proper Exception Handling
- Disable displaying runtime errors in web servers

**Use JSON or YAML
- Use JSON-based APIs instead (REST)

**Use Web Application Firewalls (WAFs)
