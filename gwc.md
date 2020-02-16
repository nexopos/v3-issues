This is the updated guide for the API endpoint for Gastro Waiter Companion.
This guide describe how to authenticate to the platform and how to perform request to the end application. The example will be made over 
an actual demo running on the domain "http://gwc.my.nexopos.com". All mention of this domain on this guide should be replaced by the equivalent
value which should be renderer dynamically (more explanations below).

## Authentication
The authentication is the process by which thanks to an QR Code, GWC can connect to a running instance of Gastro. This involve scanning a QR
Code from the application itself and performing asynchronous request on the backend to ensure the authentication.

The authentication is the main process. Every request should go through here. The server cloud return either a positive or a negative response.
According to the response, the application should provide further details to the end user which is the customer.

### Ping QR Code Validation on Platform
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
### Validating the QR Code on Gastro
This part is to ensure Gastro will stop generating QR code and close the popup related to it.
GWC should send a request to Gastro including all the data that has been returned by the platform.
At this moment, GWC should store the data provided by the QR Code.

The request send to Gastro should be as follows :

**Endpoint:** http://gwc.my.nexopos.com/api/gwc/authentication   
**Headers:**  
```
X-API-KEY: [token value get from the QR code]
Content-Type: application/json
Accept: application/json
```  
**Method:** POST  
**Data:**   
```
{
    "platform_key": "",
    "token": "",
    ...
}
```
All the data provided on the "data" parameter of the reponse (Ping QR Code Validation on Platform) should be sent to the Gastro installation as POST data. Once done, Gastro will decide or not to close and validate the authentication. However from GWC, the authentication is complete.

### Retreiving Data
The data should be collect in a periodic timespan. Retreiving data includes tables, orders with meals. If at this point 
the Gastro instance returns an error, the application should offer an option to reconnect to the user. Reconnecting involve deleting stored information and
displaying the scan code page again.

Here is how the request should be made : 

**Endpoint:** http://gwc.my.nexopos.com/api/gwc/fetch  
**Method:** GET  
**Headers:**  
```
X-API-KEY: [token value get from the QR code]
Content-Type: application/json
Accept: application/json
```
**Response:**
```json
[{
    "tables": [
        {
            "NAME": "string",
            "ID": "number",
            "STATUS": "string",
            "CURRENT_SEATS_USED": "number",
            "MAX_SEATS": "number",
            "persons": "number"
        }
    ]
    "orders" : [
       {
            "ID" : "int",
            "DATE_CREATION" : "string",
            "CODE" : "string",
            "TOTAL": "number",
            "SOMME_PERCU": "number",
            "REF_CLIENT": "number",
            "AUTHOR": "number",
            "RESTAURANT_ORDER_TYPE": "dinein | takeaway | delivery",
            "RESTAURANT_ORDER_STATUS": "pending, ongoing, ready",
            "items": [
                "ID": "number",
                "RESTAURANT_FOOD_MODIFIERS": "json",
                "RESTAURANT_FOOD_STATUS": "pending, ongoing, ready, collected",
                "PRIX": "number",
                "PRIX_TOTAL": "number",
                "TOTAL_TAX": "number",
                "NAME": "string"
            ]
       }
    ]
}]
```
