# OAuth Implementation on the relayr Platform

The core OAuth 2.0 protocol defines four main grant types - or *authorization flows*,
used for obtaining authorization. The relayr platform has implemented two of them:
**Authorization Code** and **Implicit Grant for Browser-Based Client-Side Applications**

During the registration process, when registering as an Application Publisher (developer)
you could choose which of these flows best fits your application type.
As the final stage of the registration of an app, you will receive two unique identifiers
for your app `client_id` and `client_secret`.


## Authorization Code 

In this Authorization flow, once the resource owner grants access to their data
on the resource server, they are redirected back to the application with an
authorization code as a query parameter in the URL (as a # fragment). This code is then exchanged for 
an access token by the application. This exchange is done in a server to server manner
and requires the application's *clientId* and *clientSecret*. 
This flow ensures that not even the resource owner can obtain the access token and 
hence allows for extremely long-lived tokens.

#### The Authorization Code in the relayr Context 

In the relayr context, a user wishing to grant access to an application is
redirected to a relayr page where they are prompted to enter their relayr credentials.
Once the credentials are entered, the user is redirected back to the application page
with the parameter `code` in the URL. This parameter is only valid for 5 minutes during
which the Authentication process takes place. The application-side server exchanges
the combination of the `code` parameter along with the specific application 
`client_id` and `client_secret`, for an access token, which is valid indefinitely.


## Implicit Grant for Browser-Based Client-Side Applications

In this Authorization flow, once the resource owner grants an application, access to their
data, they are automatically redirected to an application page which includes the 
access token in a # (hash) fragment, in the URL. The application can extract the token
from the hash fragment using JavaScript and use it for issuing further API requests. This flow allows for
short-lived tokens only.


#### The Implicit Grant in the relayr Context 

In the relayr context, a user wishing to grant access to an application is
redirected to a relayr page where they are prompted to enter their relayr credentials.
Once the credentials are entered, the user is redirected back to the application page
with the parameter `access_token` in the URL. The application can then
parse the URL to extract the token. This token will only be valid for two weeks.


## How does it all work?

The initial call which sets both authorization flows in motion is:

[GET] `https://api.relayr.io/oauth2/auth?redirect_uri={redirect_uri}&response_type={response_type}&client_id={clientId}&scope={scope}`

Where: 

   * `redirect_uri` (string): The URI of the page where the user is redirected upon successful login. The URI *must* include the protocol used e.g. 'http'. **The redirect URI is set when an application is registered on the relayr Platform.** 
   * `response_type` (string): The type of response to be returned according to the type of authorization initiated. The two available values are `code` and `token`. 
   * `client_id` (string): The clientId assigned to the app upon registration.
   * `scope` (string): A list of permissions the application is to be granted. List items should be separated by a space. The scope which is currently available for app developers is the _access-own-user-info_ scope. 
    
Following this call, two scenarios are expected:

### Applications utilizing The *Authorization Code*:

Once a user successfully enters their relayr credentials, they will be redirected to the URL specified under `redirect_uri`. 
This URL will also include the parameter `code` as a fragment, denoted by a `#`. For example:

`https://mycoolrelayrapp.com/hello#code=4papf.tjzRM2iLtaEaPm`

The code value is only valid for 5 minuted during which, it would need to be sent to the relayr server along with the `client_id` and `client_secret`:

[POST] `https://api.relayr.io/oauth2/token`

**For Example**

      	POST /oauth2/token HTTP/1.1
		Host: api.relayr.io
		Content-length: 219
		content-type: application/x-www-form-urlencoded
		code=hSCQRLDr.i1T9xTZw4uP&redirect_uri=https%3A%2F%2Fdevelopers.google.com%2Foauthplayground&client_id=YZMPclj.lRwF1kGvDAOIORwSvLBPviQw&scope=&client_secret=s-VqLywcrGp3r7f__1p8ddhBBI7NhomT&grant_type=authorization_code
      
Another option of sending the `client_id` and `client_secret` parameters is as authorization header parameters rather than as form parameters.

The response received is a JSON response. 

**For example**

      {
        "access_token": “your_shiny_access_token",
        "token_type":  “bearer"
      }

### Applications utilizing The *Implicit Grant*:

Once a user successfully enters their relayr credentials, they will be redirected to the URL specified under `redirect_uri`. 
This URL will also include the parameters `access_token` and `token_type` as a fragment, denoted by a `#`. For example:

`https://mycoolrelayrapp.com/hello#access_token=KyHH0r1a2Ll.uoX-m7go74FNKSNN0Svj&token_type=Bearer`   

The access token could then be parsed from the URL and used in the header of further API requests. For example:

`Bearer KyHH0r1a2Ll.uoX-m7go74FNKSNN0Svj`

### The *User Info* Call

The first authorization call which you would probably want to initiate is the 

`[GET].../oauth2/user-info` 

The call returns information about the user initiating the request.
The authentication header of the request must include the specific user access token otherwise the user information will not be returned.
