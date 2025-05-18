# Spring OAuth 2 Example

This repository provides an example of how to secure a Spring Boot application using OAuth 2.0. It demonstrates how to integrate OAuth 2.0 authentication and authorization with popular identity providers such as Google, GitHub, and custom authorization servers.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Configuration](#configuration)
  - [Running the Application](#running-the-application)
- [Usage](#usage)
- [Security Configuration](#security-configuration)
- [Customizations](#customizations)
- [Troubleshooting](#troubleshooting)
- [References](#references)
- [License](#license)

## Overview

This project uses Spring Security's OAuth 2.0 client support to enable Single Sign-On (SSO) and secure REST APIs. It can be used as a starting point for building secure web applications or RESTful services.

## Features

- OAuth 2.0 login with popular providers (e.g., Google, GitHub)
- Secures endpoints using OAuth 2.0 scopes and roles
- JWT token validation
- Logout and session management
- Easily extensible for custom OAuth 2.0 providers

## Getting Started

### Prerequisites

- Java 17 or later
- Maven 3.8+
- An OAuth 2.0 provider (e.g., Google, GitHub, or your own authorization server)

### Configuration

1. **Register your application**  
   Register your application with your chosen OAuth 2.0 provider and obtain the client ID and client secret.

2. **Set up application properties**  
   Update the `application.yml` or `application.properties` with your OAuth 2.0 credentials:

   ```yaml
   spring:
     security:
       oauth2:
         client:
           registration:
             google:
               client-id: YOUR_GOOGLE_CLIENT_ID
               client-secret: YOUR_GOOGLE_CLIENT_SECRET
               scope: openid, profile, email
             github:
               client-id: YOUR_GITHUB_CLIENT_ID
               client-secret: YOUR_GITHUB_CLIENT_SECRET
               scope: read:user
   ```

3. **Configure Redirect URI**  
   Ensure that the redirect URI matches your application settings in the OAuth provider (e.g., `http://localhost:8080/login/oauth2/code/google`).

### Running the Application

```bash
mvn spring-boot:run
```

Navigate to `http://localhost:8080` and click the login link to authenticate with your chosen provider.

## Usage

- Access the home page as an anonymous or authenticated user.
- Click "Login" to authenticate via OAuth 2.0.
- Secure endpoints (e.g., `/api/user`) require authentication and proper scopes/roles.

## Security Configuration

The security configuration is set up in `SecurityConfig.java`:

```java
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .antMatchers("/", "/login**").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2Login()
            .and()
            .logout(logout -> logout.logoutSuccessUrl("/"));
        return http.build();
    }
}
```

## Customizations

- **Add new OAuth 2.0 providers:**  
  Add additional provider configurations under `spring.security.oauth2.client.registration`.

- **Custom user details:**  
  Implement a custom `OAuth2UserService` to map provider-specific user information.

- **Role-based access:**  
  Use `@PreAuthorize` or configure access rules based on user roles or scopes.

## Troubleshooting

- **Invalid redirect URI:**  
  Ensure the redirect URI configured with your OAuth provider matches the application.

- **Missing client ID/secret:**  
  Double-check your credentials in the configuration file.

- **403 Forbidden:**  
  Verify that the user has the required scopes/roles to access secured endpoints.

## References

- [Spring Security OAuth 2.0 Docs](https://docs.spring.io/spring-security/reference/servlet/oauth2/index.html)
- [Spring Boot Documentation](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
- [OAuth 2.0 Spec](https://oauth.net/2/)

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
