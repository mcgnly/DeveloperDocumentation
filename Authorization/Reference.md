# Authorization API
The relayr authorization mechanism utilizes the OAuth 2.0 standard.
The relayr Authorization Doc provides information about the two OAuth methods implemented on the relayr platform.

# A Few Words about OAuth
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

**Authorization Server** - The authorization server receives concent from the resource
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

For further reading about OAuth, please visit - [http://tools.ietf.org/html/rfc6749](http://tools.ietf.org/html/rfc6749)

### OAuth on the relayr Platform

The core OAuth 2.0 protocol defines four main grant types - or *authorization flows*,
used for obtaining authorization. The relayr platform has implemented two of them:
**Authorization Code** and **Implicit Grant for Browser-Based Client-Side Applications**

During the registration process, when registering as an Application Publisher (developer)
you could choose which of these flows best fits your application type.
As the final stage of the registration of an app, you will receive two unique identifiers
for your app `client_id` and `client_secret`.


# Authorization Flows 

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
parse the URL to exctract the token. This token will only be valid for two weeks.


# How does it all work?

The initial call which sets both authorization flows in motion is:

[GET] `http://api.relayr.io/oauth2/auth/?redirect_uri={redirect_uri}&response_type={response_type}&clientId={clientId}&scope={scope}`

Where: 

   * `redirect_uri` (string): The URI of the page where the user is redirected upon successful login.
   * `response_type` (string): The type of response to be returned according to the type of authorization innitiated. The two             available values are `code` and `token`. 
   * `client_id` (string): The clientId assigned to the app upon registration.
   * `scope` (string): A list of permissions the application is to be granted. List items should be separated by a space.
    
Following this call, two scenarios are expected:

#### Applications utilizing The *Authorization Code*:

Once a user sucessfully enters their relayr credentials, they will be redirected to the URL specified under `redirect_uri`. 
This URL will also include the parameter `code` as a fragment, denoted by a `#`. For example:

`https://mycoolrelayrapp.com/hello#code=4papf.tjzRM2iLtaEaPm`

The code value is only valid for 5 minuted during which, it would need to be sent to the relayr server along with the `client_id` and `client_secret`:

[POST] `http://api.relayr.io/oauth2`

      { 
        "client_id" = "{client_id}",
        "client_secret" = "{client_secret}",
        "code" = "4papf.tjzRM2iLtaEaPm" 
      }
      
Another option of sending the `client_id` and `client_secret` parameters is as authorization header parameters rather than form parameters.

The response received is a JSON responnse. For example:

      {
        "access_token": “your_shiny_access_token",
        "token_type":  “bearer"
      }

#### Applications utilizing The *Implicit Grant*:

Once a user sucessfully enters their relayr credentials, they will be redirected to the URL specified under `redirect_uri`. 
This URL will also include the parameters `access_token` and `token_type` as a fragment, denoted by a `#`. For example:

`https://mycoolrelayrapp.com/hello#access_token=KyHH0r1a2Ll.uoX-m7go74FNKSNN0Svj&token_type=Bearer`   

The access token could then be parsed from the URL and used in the header of further API requests. For example:

`Bearer KyHH0r1a2Ll.uoX-m7go74FNKSNN0Svj`

## A Short OAuth Example

The example below provides a step by step interactive demonstration of the process which takes place when OAuth authorization tokens are issued on the relayr platform.

### The Access Code Scenario

#### 1. Access the [OAuthPlayGround URL](https://developers.google.com/oauthplayground/#step1&scopes=access-own-user-info&auth_code=sBCSndPuYsTLqxsPHz_f&access_token_field=_JP_JurFCl9xfVF3MZP_k5JZTL_MdPUy&url=https%3A//api.relayr.io/oauth2/user-info&content_type=application/json&http_method=GET&useDefaultOauthCred=unchecked&oauthEndpointSelect=Custom&oauthAuthEndpointValue=http%3A//api.relayr.io/oauth2/auth&oauthTokenEndpointValue=http%3A//api.relayr.io/oauth2/token&oauthClientId=YZMPclj.lRwF1kGvDAOIORwSvLBPviQw&oauthClientSecret=s-VqLywcrGp3r7f__1p8ddhBBI7NhomT&access_token_issue_date=1400774322&for_access_token=_JP_JurFCl9xfVF3MZP_k5JZTL_MdPUy&includeCredentials=checked&accessTokenType=bearer&autoRefreshToken=unchecked&accessType=offline&forceAprovalPrompt=checked&response_type=code) 


#### 2. The context of the action is predefined:

![](assets/step1.jpg)

#### 3. Click the settings menu on the top right corner.

![](/assets/step2.jpg)

#### 4. Click 'Authorize APIs'.

![](/assets/step3.jpg)

#### 5. You will be prompted to enter your relayr User credentials.

![](/assets/step4.jpg) 

#### 6. Enter your credentials and click to sign in.

![](/assets/step5.jpg)

#### 7. Click Exchange Authorization code for tokens.


![](/assets/step6.jpg)

#### 8. The token is received as a JSON snippet in the response.


![](/assets/step7.jpg)

### The Implicit Grant Scenario

#### 1. Access the [OAuthPlayGround URL](https://developers.google.com/oauthplayground/#step1&scopes=access-own-user-info&auth_code=sBCSndPuYsTLqxsPHz_f&access_token_field=_JP_JurFCl9xfVF3MZP_k5JZTL_MdPUy&url=https%3A//api.relayr.io/oauth2/user-info&content_type=application/json&http_method=GET&useDefaultOauthCred=unchecked&oauthEndpointSelect=Custom&oauthAuthEndpointValue=http%3A//api.relayr.io/oauth2/auth&oauthTokenEndpointValue=http%3A//api.relayr.io/oauth2/token&oauthClientId=YZMPclj.lRwF1kGvDAOIORwSvLBPviQw&oauthClientSecret=s-VqLywcrGp3r7f__1p8ddhBBI7NhomT&access_token_issue_date=1400774322&for_access_token=_JP_JurFCl9xfVF3MZP_k5JZTL_MdPUy&includeCredentials=checked&accessTokenType=bearer&autoRefreshToken=unchecked&accessType=offline&forceAprovalPrompt=checked&response_type=code) 


#### 2. The context of the action is predefined:


#### 3. Click the settings menu on the top right corner. Select the Client Side scenario from the drop down 

![](/assets/client1.jpg)


#### 4. Click 'Authorize APIs'.

![](/assets/step3.jpg)

#### 5. You will be prompted to enter your relayr User credentials.

![](/assets/step4.jpg) 

#### 6. Enter your credentials and click to sign in. 

![](/assets/step5.jpg)


#### 7. The token is received as part of the URL and on the left hand side

![](/assets/client2.jpg)
