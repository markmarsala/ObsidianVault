## REST - Representational State Transfer
- GET
- POST
- PUT
- DELETE

```
GET /users/123
```


## Simple Object Access Protocol (SOAP)
```
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:GetStockPrice>
         <tem:StockName>AAPL</tem:StockName>
      </tem:GetStockPrice>
   </soapenv:Body>
</soapenv:Envelope>
```


## GraphQL
```
query {
  user(id: 123) {
    name
    email
  }
}
```

