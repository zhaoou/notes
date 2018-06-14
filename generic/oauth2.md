## Definitions

### Resource owner

* person who owns the data in some system

### Resource server

* server with data client app want to access

### Client app

* app that wants to access that data
* wants data access token
* has redirect URI
* gets auth code and exchanges it for access token

### Auth server

* ex. accounts.google.com
* gives auth code
* has a list of scopes(permissions)

## Flows

### Authorization code flow:

* Client app obtained client_id and secret_key in advance

```

CLIENT APP                                         AUTH SERVER    RESOURCE OWNER
  |                                                   |
  |  -------(client_id, callback, scopes)------->>>   |             
  |                                                   |   ----> Allow scopes?
  |                                                   |   <---- Yes
  |                                                   |
  |  <<<----(callback with auth_code)--------------   |
  |                                                   |                FRONT CHANNELS ABOVE
----------------------------------------------------------------------------------------------  
  |                                                   |                BACK CHANNELS BELOW 
  |  -------(secret_key, auth_code)------------->>>   |                                                    
  |                                                   |      
  |                                                   |      
  |  <<<----(access_token(bearer), scopes)---------   |
  |                                                   |
__|___________________________________________________|
   \
     \
(token with scopes)
        \  
         |
         |
         |
  RESOURCE SERVER

```


### Implicit flow

* no back-end
* no back channels
* get token right away
* ex. SPA

### Resource owner password credentials flow

* back channels only

### Client credentials flow

* back channels only

## Oauth2 is for Authorization

* can't get user info
* no common scopes

## OpenID Connect is for Authentication

* OpenID was added to OAuth2 to provide authentication
* adds id_token in JWT with user info
* user_info endpoint to get additional information
* standardised scopes
* standardised implementation



