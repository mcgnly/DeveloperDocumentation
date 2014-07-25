# Introduction to OAuth 

*OAuth* is an open standard for authorization.

Prior to explaining the main concept of OAuth, here are a few key roles
necessary for understanding the standard:

**Resource Server** - The server hosting user-owned resources such as photos, 
calendars, videos and contacts.

**Resource Owner** - Essentially the user of an application. In the OAuth context
The resource owner is able to grant access to their own data,
stored on the resource server.

**Client** - An Application. In the OAuth context, this application would issue API
requests in order to perform actions on protected resources on 
behalf of the resource owner.

**Authorization Server** - The authorization server receives consent from the resource
owner and in return issues access tokens to clients, enabling them to access protected
resources hosted by the resource server.


OAuth provides clients *'secure delegated access'* to a user's server
resources on behalf of the user. 
It specifies a process for the owners of the resources, to authorize
third-party access to their resources without having to share their credentials.


Instead of using the explicit credentials of the resource owner, OAuth allows for *access
tokens* to be issued to third-party clients by an authorization server. 
the latter are issued upon approval from the resource owner.
The client then uses the access token to access the protected resources
hosted by the resource server. 

OAuth is commonly used as a way for
users to log into third party web sites using their Google,
Facebook or Twitter accounts, without worrying about their access
credentials being compromised. 


For further reading about OAuth, please visit - <a href="http://tools.ietf.org/html/rfc6749" target="_blank"> http://tools.ietf.org/html/rfc6749 </a>

