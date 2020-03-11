# Mivation - Integration Gateway

The Mivation solutions support push of data via a flexible integration-gateway that allows mapping of payloads to our internal APIs.

- All data is pushed through HTTPS POST of "text/json" or "application/json" to our API.
- All calls are authenticated via Basic authentication or Bearer tokens.
- Each gateway push can contain from 1 to 10,000 records of data.
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
- The "format" in the post is assigned by Mivation during integration planning with the credentials.
- The is an optional callback that will be posted to a web-hook handler on your end when the message has been processed and/or if there is a problem processing the message
- Large payloads will be split into 100 record fragments for processing and will send one callback per fragment.


----

![Flow](/Gateway-Flow.png "Gateway Flow")

----


### Example Request
```
POST /push HTTP/1.1
Host: gateway.mivation.com
Connection: keep-alive
Content-Length: <body-length>
Authorization: Bearer <my-token>
Content-Type: application/json
Accept: application/json, text/json

{
  "request_id": "e4177bc770ca",
  "format": "quote_aa_204",
  "payload": [
    {
      "id": "ae15dec5-ab03-467b-8600-6d2413a2eba2",
      "event_type": "quote_complete",
      "user": "someone@somecompany.com",
      "product": "my product",
      "quote_ref_number": "3546853214"
    },
    {
      …
    }
  ]
}
```

In the above example:
- The "request_id" is any string identifier supplied by you for the transaction.  This will be echoed back on the callback (optional).  It will also be used for ensuring single-processing of transaction.
- The "format" is the format of the payload records based on the agreed upon data interchanges for your use-case.
- the "payload" contains 1-10,000 records of data.

----

### Example Response
```
HTTP/1.1 202 Accepted
Content-Length: <body-length>
Content-Type: application/json

{ "id": "ff68d5fd-ae6d-4349-9f71-3641f8b29043" }
```
In the above example:
- The "id" is a unique identifier supplied by Mivation for the transaction.  This also will be echoed back on the callback (optional).  It can also be used for reconciliation.

----

### Example Callback
```
POST /callback HTTP/1.1
Host: server.yourcompany.com
Content-Length: <body-length>
Content-Type: application/json
Accept: application/json, text/json

{
  "id": "ff68d5fd-ae6d-4349-9f71-3641f8b29043",
  "request_id": "e4177bc770ca",
  "results": [
    {
      "index": 0
      "id": "ae15dec5-ab03-467b-8600-6d2413a2eba2",
      "result": "OK"
    },
    {
      "index": 1
      "id": "ae15dec5-ab03-467b-8600-6d2413a2eba3",
      "result": "ERROR",
      "error": "[1003] Missing field: 'user'"
    }
  ]
}
```

In the above example:
- The "id" is a the unique idenitifier supplied by Mivation for the original transaction, in the HTTPS response.
- The "request_id" is the string idenitifier supplied by you for the original transaction.
- the "results" contains 1-100 results, matching the original transaction.  Each result will refer to the index posiion from the original request as well as echo back any "id" supplied with the original request item.  The "result" and possible "error" elements indicate success ro failure of porcessing for each record and a possible failure reason.
