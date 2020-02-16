This is the updated guide for the API endpoint for Gastro Waiter Companion.
This guide describe how to authenticate to the platform and how to perform request to the end application. The example will be made over 
an actual demo running on the domain "http://gwc.my.nexopos.com". All mention of this domain on this guide should be replaced by the equivalent
value which should be renderer dynamically (more explanations below).

## Authentication
The authentication is the process by which thanks to an QR Code, GWC can connect to a running instance of Gastro. This involve scanning a QR
Code from the application itself and performing asynchronous request on the backend to ensure the authentication.

The authentication is the main process. Every request should go through here. The server cloud return either a positive or a negative response.
According to the response, the application should provide further details to the end user which is the customer.

### Validating a QR Code
The QR code request once scanned should return a JSON having the following shape : 

```json
{
    "id":"2",
    "token":"Ewjr42IbTqkaPt0HhhrztpyTPw9JJCO7k2NiG1W7",
    "platform_key":null,
    "domain":"http:\/\/gwc.my.nexopos.com\/"
}
```
The application should then foward all the details to the platform using the following request shape.

**Endpoint:** https://platform.nexopos.com/api/gwc/v1/authentication
**Headers:**  
```
X-APP-CLIENT: gwc
Accept: application/json
Content-Type: application/json
```
**Method:** POST
**Data:**
```json
{
    "id":"2",
    "token":"Ewjr42IbTqkaPt0HhhrztpyTPw9JJCO7k2NiG1W7",
    "platform_key":null,
    "domain":"http:\/\/gwc.my.nexopos.com\/"
}
```
**Response 200(Positive)**
```json
{
  "status": "success",
  "message" : "...",
  "data": []
}
```
**Response 403(Negative)**
The data parameter could contain further details about the error.
```json
{
  "status": "failed",
  "message" : "...",
  "data": []
}
```
