What is the hardcoded password for the database connection in the MultimasterAPI.dll file?

```
cd C:\
Get-ChildItem -Path C:\ -Recurse -File -ErrorAction SilentlyContinue | Where-Object { $_.Name -ieq "MultimasterAPI.dll" }
```

We can debug it with dnSpy: https://github.com/0xd4d/dnSpy

MultimasterAPI.Controllers -> ColleagueController reveals credentials
