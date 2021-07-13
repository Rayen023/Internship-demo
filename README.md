# Keycloak Setup Documentation 
OIDC : adds identity layer on top of oauth 2 => Resource server now receives Access Token + ID Token ( holds info about User like Email , Name ,...)
Authorisation avec un code : Authorization Code Grant

## PART 1 : SETUP Keycloak : authorization server
* Download keycloak 14.0.0
* Launch ./bin/standalone.bat -Djboss.http.port=8081 (8000)
* Create initial admin :
    * username : amdin
  * password : 123456
* master realm : manage set of clients 
  , users
. Each user is specific to a realm

* Add realm : oauth2-demo-realm
(service side render)
* Create client : oauth2-demo-thymeleaf-client
    * access type : confidential 
  * redirect URL : http://localhost:8080/login/oauth2/code/oauth2-demo-thymeleaf-client
  * -> Credentials -> Regenerate secret

* Create User : Username : oauth2-demo-user
Password : 123456

* Create app https://start.spring.io/
### Added dependencies  
  
  `<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
`

### Application.properties :
`server.port=8080

spring.security.oauth2.client.registration.oauth2-demo-thymeleaf-client.client-id=oauth2-demo-thymeleaf-client
spring.security.oauth2.client.registration.oauth2-demo-thymeleaf-client.client-secret=2273e28b-3010-4d97-97b8-baa518a330e6
spring.security.oauth2.client.registration.oauth2-demo-thymeleaf-client.scope=openid, profile, roles
spring.security.oauth2.client.registration.oauth2-demo-thymeleaf-client.authorization-grant-type=authorization_code
spring.security.oauth2.client.registration.oauth2-demo-thymeleaf-client.redirect-uri=http://localhost:8080/login/oauth2/code/oauth2-demo-thymeleaf-client

spring.security.oauth2.client.provider.oauth2-demo-thymeleaf-client.issuer-uri=http://localhost:8180/auth/realms/oauth2-demo-realm

issuer-uri : realm settings (keycloak server) -> Endpoints : OpenID Endpoint  Configuration
(instead of providing all endpoints (token_endpoint, authorization_endpoint , etc ) we can add only the issuer url )
`
added template (home.html) + controllers : HomeController and HomeRestController



## PART 2 : Adding github SSo:


* keycloak server -> identity providers -> add (GitHub, facebook, twitter…)
* Register app in GitHub or facebook … with the redirect url provided (.../github/endpoint)
* https://github.com/settings/developers
* Then add the generated ClientID and secret in keycloak
* Save
