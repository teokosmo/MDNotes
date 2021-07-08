# OAuth2 #

## Description

OAuth2 is a protocol enabling a Client application, often a web application, to act on behalf of a User, but with the User’s permission. The actions a Client is allowed to perform are carried out by a Resource Server (another web application or web service), and the User approves the actions by telling an Authorization Server that he trusts the Client to do what it is asking. Clients can also act as themselves (not on behalf of a User) if they are permitted to do so by the Authorization Server.  
The most common way for a Client to present itself to a Resource Server is using a bearer token, as covered in the core OAuth2 specification. The token is obtained from the Authorization Server, with the User’s approval if necessary, and stored by the Client. Then when it needs to access a Resource Server the Client sends a special HTTP header in the form
```
Authorization: Bearer <TOKEN_VALUE>
```
The token value is opaque to a client, but can be decoded by a Resource Server so it can check that the Client and User have permission to access the requested resource.  
OAuth2 is, at its heart, an authentication protocol for lightweight services, which are Resource Servers in the domain language of the specification. A Client application that wants to access a protected resource sends an authorization header, a bit like in the Basic authentication case.


## References

* http://www.bubblecode.net/en/2016/01/22/understanding-oauth2/
* https://www.cloudfoundry.org/blog/oauth-rest/