# Mivation - Integration Gateway

The Mivation solutions support push of data via a flexible integration-gateway that allows mapping of payloads to our internal APIs.

- All data is pushed through HTTPS POST of "text/json" or "application/json" to our API.
- All calls are authenticated via Basic authentication or Bearer tokens.
- Each gateway push can contain from 1 to 10,000 records of data, with a maximum size of 10MB.
- The format of the records is fluid with the exception of the envelope (see below)
- Our server will generally return one of the following responses
  - 202 – Accepted
    - This is a success response that means the transaction was accepted and queued for processing
    - It will include "application/json" content with a transaction id from our server.
  - 204 - No Content
    - The request was properly formatted except for an empty payload, the request was discarded.
  - 400 – Bad Request
    - The request was improperly formatted, the payload was discarded
  - 401 – Unauthorized
    - The request came without authentication or with invalid credentials
- Our server queues all request using enterprise grade queuing technology.  You can expect all requests to be turned around by our server sub-second.
- The `format` in the post is assigned by Mivation during integration planning with the credentials.
- The is an optional `id` for each payload record that is *required* for updatable data.
- Payloads may be split smaller fragments for processing. 


----

![Flow](/Gateway-Flow.png "Gateway Flow")

----

## API Endpoints

**Sandbox / Testing**  
https://gateway.sandbox.mivation.com/push

**Production**  
https://gateway.mivation.com/push

*Both of the above endpoints accept only HTTPS POST requests with a content-type of "text/json" or "application/json".  Attempting to visit them in your browser will usually result in a 405 error.*

----

### Example Request
```yaml
POST /push HTTP/1.1
Host: gateway.mivation.com
Connection: keep-alive
Content-Length: <body-length>
Authorization: Bearer <my-token>
Content-Type: application/json
Accept: application/json, text/json

{
  "format": "activity",
  "request_id": "bc314d38-f4a8badc29dd",
  "payload": [
    { 
      "id": "02784e36-e70f4c86595b",
      "type": "call",
      "timestamp": "2020-05-18T21:15:00-5:00",
      "duration": "PT25M30S",
      "user_email": "jonny.test@mivation.com",
      "contact": "+18774289403",
      "direction": "outbound"
    },
    { 
      "id": "25b8d74f-3becb0ede39f",
      "type": "quote",
      "timestamp": "2020-05-18T21:20:00-5:00",
      "user_email": "jonny.test@mivation.com",
      "contact": "Jane S",
      "product_line": "fire",
      "product": "Renter",
      "amount": 123.45,
      "href" : "https://my.quotes.net/25b8d74f-3becb0ede39f"
    }
  ]
}
```

In the above example:
- The `format` is the format of the payload records based on the agreed upon data interchanges for your use-case.
- The `payload` contains 1-10,000 records of data.
- The *optional* `request_id` is any string identifier supplied by you for the transaction to be used for request tracing.

----

### Example Response
```yaml
HTTP/1.1 202 Accepted
Content-Length: <body-length>
Content-Type: application/json

{ "id": "ff68d5fd-ae6d-4349-9f71-3641f8b29043" }
```
In the above example:
- The `id` is a unique identifier supplied by Mivation for the transaction, that can be used for reconciliation.  We recommend recording this value when possible.	




