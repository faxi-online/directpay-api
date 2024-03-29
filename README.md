# DirectPay API
DirectPay is a service provided by Faxi platform that allows merchant
create and receive payment with simple Rest API.

## Create DirectPay
To create a new payment you should do a post following the specification below.

### URL
In test you must do post to the URL below
```
https://test.faxi.online/api/direct-pay
```

### Headers
To authenticate with the API you should send
the token provided by Faxi in HTTP Headers
- Authorization: Bearer token
- Content-Type: application/json

### Body
The HTTP body should be a JSON with those properties:
- *internal_reference* - the identifier to your internal transaction id
- *amount* - the amount of the payment
- *callback_url* - the endpoint of your Webhook to receive the notify
when the payment is done successfully
```json
{
	"amount": "12",
	"callback_url": "https://jsonplaceholder.typicode.com/posts",
	"internal_reference": "ref123"
}
```
You can send more data in the request body
```json
{
    "amount": "1000",
    "callback_url": "https://jsonplaceholder.typicode.com/posts",
    "internal_reference": "ref124",
    "description": "Pay my service ABC",
    "email": "client@mail.com"
}
```

### Response
The response will be a JSON with those properties:
- *success* - a boolean value that say if the transaction is created successfully or not
- *message* - a description of the action done, it will be useful in case of errors
- *id* - the id of DirectPay created in Faxi
- *transaction_url* - the link that you should redirect you user to do the payment
```json
{
    "success": "true",
    "message": "Create direct pay successfully",
    "id": 1,
    "transaction_url": "https://test.faxi.online/process-payment/118/7vG9KH3ZA8ENQT01"
}
```

### Webhook
When the payment is done successfully your Webhook
will receive a POST with JSON data that contains those properties:
- *id* - the id of DirectPay created in Faxi
- *internal_reference* - the identifier to your internal transaction id
- *status* - the status of payment where 1 = success, 0 = pending
- *payed_at* - when the payment was done
```json
{
	"id": "12",
	"internal_reference": "ref123",
	"status": "1",
	"paid_at": "2022-03-03 19:42:44"
}
```

## Get DirectPay status
You can get the status of your transaction doing a GET request following the specification below.

### URL
In test you must do GET to the URL below,
where 1 is the DirectPay id created in Faxi.
```
https://test.faxi.online/api/direct-pay/1
```

### Headers
To authenticate with the API you should send
the token provided by Faxi in HTTP Header
- Authorization: Bearer token

### Response
```json
{
    "success": "true",
    "message": "DirectPay object Found",
    "id": 1,
    "internal_reference": "ref123",
    "status": 0,
    "paid_at": null,
    "description": "Pay my service ABC",
    "email": "client@mail.com",
    "transaction_url": "https://test.faxi.online/process-payment/118/7vG9KH3ZA8ENQT01"
}
```
