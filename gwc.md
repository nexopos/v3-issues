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

```
