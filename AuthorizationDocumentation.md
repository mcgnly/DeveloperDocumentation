# Authorization API
The relayr Authorization mechanism utilizes the OAuth 2.0 standard.
The relayr Authorization API provides programmatic acces to the two OAuth 2.0 
Authorization methods implemented on the relayr platform.

# A Few Words about OAuth
*OAuth* is an open standard for authorization.

Prior to explaining the main concept of OAuth, here are a few key concepts
necessary for understanding the standard:

**Resource Server** - The server hosting user-owned resources such as photos, 
calendars, videos and contacts.

**Resource Owner** - Essentially the user of an application. In the OAuth context
The resource owner is able to grant access to their own data,
stored on the resource server.

**Client** - An Application. In the OAuth context, this application would issue API
requests in order to perform actions on protected resources on 
behalf of the resource owner

**Authorization Server** - The authorization server receives concent from the resource
owner and in return issues access tokens to clients, enabling them to access protected
resources hosted by the resource server.


OAuth provides clients *'secure delegated access'* to a user's server
resources on behalf of the user. 
It specifies a process for the owners of the resources, to authorize
third-party access to their resources without having to share their credentials.


Instead of using the explicit credentials of the resource owner, OAuth allows *access
tokens* to be issued to third-party clients by an authorization server, 
upon approval from the resource owner.
The client then uses the access token to access the protected resources
hosted by the resource server. 

OAuth is commonly used as a way for
users to log into third party web sites using their Google,
Facebook or Twitter accounts, without worrying about their access
credentials being compromised. 

For further reading about OAuth, please visit - `/link to be added`

### OAuth on the relayr Platform

The core OAuth 2.0 protocol defines four main grant types - or *authorization flows*,
used for obtaining authorization. The relayr platform has implemented two of them:
**Authorization Code** and **Implicit Grant for Browser-Based Client-Side Applications**

During the registration process, when registering as an Application Publisher (developer)
you could choose which of these flows best fits your application type.
As the final stage of the registration of an app, you will receive two unique identifiers
for your app `clientId` and `clientSecret`.


# Authorization Flows 

## Authorization Code 

In this Authorization flow, once the resource owner grants access to their data
on the resource server, they are redirected back to the application with an
authorization code as a query parameter in the URL. This code is then exchanged for 
an access token by the application. This exchange is done in a server to server manner
and requires the application's *clientId* and *clientSecret*. 
This flow ensures that not even the resource owner can obtain the access token and 
hence allows for extremely long-lived tokens.

### Authorization Code in the relayr Context 

In the relayr context, a user wishing to grant access to an application will be
redirected to a relayr page where they would be prompted to enter their relayr credentials.
Once the credentials are entered, the user is redirected back to the application page
with the parameter `code` in the URL. This parameter is only valid for 5 minutes during
which the Authentication process takes place. The application side server exchanges
the combination of the `code` parameter along with the specific application 
`clientId` and `clientSecret` for an access token which is valid indefinitely.


## Implicit Grant for Browser-Based Client-Side Applications

In this Authorization flow once the resource owner grants an application access to their
data, they are automatically redirected to an application page which includes the 
access token in a #hash fragment in the URL. The application can extract the token
from the hash fragment using JavaScript, and send API requests. This flow allows for
short-lived tokens only.


### The Implicit Grant in the relayr Context 

In the relayr context, a user wishing to grant access to an application will be
redirected to a relayr page where they would be prompted to enter their relayr credentials.
Once the credentials are entered, the user is redirected back to the application page
with the parameter `accessToken` in the URL. The application The application can then
parse the URL to exctract the clientToken. This token will only be valid for two weeks.


#### The initial URL which sets both Authorization flows in motion is:

http://api.relayr.io/oauth2/auth/?redirect_uri={redirect_uri}&response_type={response_type}&clientId={clientId}&scope={scope} 

Where: 

    - **redirect_uri** (string):  A correctly formed URI of the page where the user is redirected upon successful login.
    - **response_type** (string):  The type of response to be returned according to the type of authorization innitiated 
    - **clientId** (string): The clientId assigned to the app upon registration
    - **scope** (string, ): 
    


