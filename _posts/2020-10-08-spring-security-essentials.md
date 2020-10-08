# Spring Security Essentials

## Normal

### Components

```
Authentication
  Principal
  Credential
  Authority
Cookie && HttpSession
```

### Main architecture
```
Filter -> AuthenticationManager -> AuthenticationProvider +-> UserDetailService
                                                          |
                                                          +-> PasswordEncoder
```

## OAuth2

Spring Security OAuth implemented the OAuth2.0 flow. The project is deprecated and will be migrate to Spring Security.

### Components

* Authorization Server: Providing services for user authentication and authorization. Issuing `access_token` is the main functionality.

* Resource Server: Providing resources that need corresponding authorities. For example, UserInfo server. Resource Server need clients providing `access_token` for authentication purpose.

* Client: Who wants to access resources from Resource Server. Will be redirected to Authorization Server to get `access_token`.


## Learning Resources

* Spring Security OAuth project doc. https://projects.spring.io/spring-security-oauth/docs/oauth2.html

* Videos lessons for Spring security. https://www.youtube.com/watch?v=Of4HFbsPKqk&list=PLEocw3gLFc8XRaRBZkhBEZ_R3tmvfkWZz