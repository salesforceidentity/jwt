NOTE: Apex now has native support for RSA based JWT generation: [https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_class_Auth_JWS.htm#apex_class_Auth_JWS]


jwt
===

Apex implementation of JWT and JWT Bearer flow.   Requires Summer 14 release for RSA-SHA256 support.


#Unsigned JWT
```
JWT jwt = new JWT('none');
jwt.iss = 'your issuer';
jwt.sub = 'some subject';
jwt.aud = 'some audience';
token = jwt.issue();
```

#HMAC256 Signed JWT
```
JWT jwt = new JWT('HS256');
jwt.privateKey = 'base64 encoded secret';
jwt.iss = 'your issuer';
jwt.sub = 'some subject';
jwt.aud = 'some audience';
token = jwt.issue();        
```

#RSA256 Signed JWT with PEM encoded p12
```
JWT jwt = new JWT('RS256');
jwt.pem = 'MIICXQIBAAKBgQC4U4Bma7kKa0CLU...pem encoded p12 RSA Key';
jwt.iss = 'your issuer';
jwt.sub = 'some subject';
jwt.aud = 'some audience';
token = jwt.issue();     
```

#RSA256 Signed JWT with Certificate from Setup 
```
JWT jwt = new JWT('RS256');
jwt.cert = 'JWTKey';
jwt.iss = 'your issuer';
jwt.sub = 'some subject';
jwt.aud = 'some audience';
token = jwt.issue();     
```

#Change the default expiration
By default expiration is 5 minutes (300 seconds).   Change it by passing in a validFor in seconds.  

```
JWT jwt = new JWT('none');
jwt.validFor = 60;
```

#Bearer Flow
Use the JWT bearer flow for Server to Server applications.  

```
JWTBearerFlow.getAccessToken('token_endpoint', jwt);
```

#Salesforce RSA-256 JWT Bearer Flow
[http://help.salesforce.com/HTViewHelpDoc?id=remoteaccess_oauth_jwt_flow.htm&language=en_US]

```
JWT jwt = new JWT('RS256');
jwt.cert = 'JWTKey';
jwt.iss = '3MVG9PhR6g6B7ps6TYoM9J8TuRwyvkAmDUKainDupyG6eJ92nmK8m4LYueD5Lgtnyv0QoWBrB.YjuWCVj_rl_';
jwt.sub = 'summer@cmort.org';
jwt.aud = 'https://login.salesforce.com/services/oauth2/token';
String access_token = JWTBearerFlow.getAccessToken('https://login.salesforce.com/services/oauth2/token', jwt);
 ```     

#Google RSA-256 JWT Bearer Flow
[https://developers.google.com/accounts/docs/OAuth2ServiceAccount]

```
JWT jwt = new JWT('RS256');
jwt.pem = 'MIICXQIBAAKBgQC4U4Bma7kKa0CLU...pem encoded p12 RSA Key';
jwt.iss = 'someclient@developer.gserviceaccount.com';
jwt.sub = 'someuser@some.domain';
jwt.aud = 'https://accounts.google.com/o/oauth2/token';
Map<String,String> claims = new  Map<String,String>();
claims.put('scope','https://www.googleapis.com/auth/drive');
jwt.claims = claims;
String access_token = JWTBearerFlow.getAccessToken('https://accounts.google.com/o/oauth2/token', jwt);
```
