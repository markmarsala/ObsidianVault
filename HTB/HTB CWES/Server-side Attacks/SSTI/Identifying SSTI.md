These special characters should make the application throw an error if it is vulnerable:
```
${{<%[%'"}}%\.
```


1. Start with this payload:
```
${7*7}
```
- fails, then try {{7*\7}}, fails, then not vulnereable, succeeds try {{7*'7'}}, if succeeds then it is jinja2 or twig, if not then it is unknown
- succeeds, then try a{\*comment*}b, if success then smarty, if fails then try ${"z".join("ab")}, if succeeds then Mako, if fails then unknown 

<img width="1440" height="943" alt="image" src="https://github.com/user-attachments/assets/7f6b6573-0738-4776-9f5c-6590056ad8cd" />

Twig will return 49 while jinja2 returns 777777.
