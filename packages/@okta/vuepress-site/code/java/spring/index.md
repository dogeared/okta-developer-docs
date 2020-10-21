---
title: Add Authentication to Your Spring Boot App
language: Java
integration: back-end
icon: code-spring
meta:
  - name: description
    content: Our guide details how to add user authentication to your Java Spring app.
---

* [<i class='icon code-spring-32'></i><span>Spring</span>](/code/java/spring/)
* [<i class='icon code-java-32'></i><span>Java</span>](/code/java/)
{.language-tabs}

## Get Started with Spring + Okta

### 1. What is Okta

Okta is an Identity Access Management platform. Okta manages the security of your users.

This means that you can offload authentication to Okta so you can focus on the business logic of what your building.

Work on the following sections to quickly integrate authentication into your app. Feel free to skip over any sections you already know about by clicking on the navigation to the right.

> **NOTE:** Okta is much, much more than authentication, but this is the best place to start. There's a whole section on learning more below.

### 2. Create an Okta Account

New to Okta? Follow these instructions to get set up.

1. [Create A Free Account](https://developer.okta.com/signup/){.Button--red data-proofer-ignore .card}
2. Fill out the form on the tab that opens up.
3. Click the link you receive in your email to set a password.
4. Come back here to continue...

### 3. Create an Okta App for Authentication

1. Navigate to **Applications** and click **Add Application**
2. Click **Web** and click **Next**
3. Give it a **Name** and click **Done**
4. Note the **Client ID** and **Client Secret**. You'll need them later.

> **NOTE:** You just created an OpenID Connect App. Don't know what that is? Don't worry - you don't need to know yet. If you want to find out more now, check out [this guide](/docs/concepts/oauth-openid/){target="_blank" rel="noopener noreferrer"}.

### 4. Build Okta into a Spring Boot App

1. You'll be working with a Spring Boot sample from [this github repo](https://github.com/okta/samples-java-spring).
2. From your terminal, execute the following:

	```bash
	git clone https://github.com/okta/samples-java-spring
	cd samples-java-spring/okta-hosted-login
	mvn -Dokta.oauth2.issuer=https://{yourOktaDomain}/oauth2/default \
		-Dokta.oauth2.clientId={clientId} \
		-Dokta.oauth2.clientSecret={clientSecret} \
		spring-boot:run
	```

	> **NOTE:** Putting secrets on the command line should ONLY be done for examples, do NOT do this in production.
3. In your browser, navigate to: `http://localhost:8080`. Login with the user you set up with Okta.

### 5. A Look at the Spring Boot Code

Here's a snippet from the Spring Security configuration:

```java
@Configuration
static class WebConfig extends WebSecurityConfigurerAdapter {
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http.authorizeRequests()
			// allow antonymous access to the root page
			.antMatchers("/").permitAll()

			// all other requests
			.anyRequest().authenticated()

			// set logout URL
			.and().logout().logoutSuccessUrl("/")

			// enable OAuth2/OIDC
			.and().oauth2Client()
			.and().oauth2Login();
	}
}
```

This is standard Spring Security configuration. Because of the deep integration the Okta Spring Boot Starter has with Spring Security, there is nothing Okta specific here.

Here's a snippet from the `home.html` template:

```html
...
<div th:if="${#authorization.expression('isAuthenticated()')}" class="text container">
	<p>Welcome home, <span th:text="${#authentication.principal.attributes['name']}">Joe Coder</span>!</p>
	...
</div>
...
```

Again, this is leveraging the regular [SpEL](https://docs.spring.io/spring-framework/docs/3.0.x/reference/expressions.html) constructs to (a) detect if a user is authenticated and (b) if so, show the user's name

## Learn More About Okta

### Recommended Guides 

* [Okta Authentication How To Guide](/docs/guides/sign-into-web-app/springboot/before-you-begin/){target="_blank" rel="noopener noreferrer"}
* [Social Login](/docs/concepts/social-login/){target="_blank" rel="noopener noreferrer"}
* [Validate access tokens](/docs/guides/validate-access-tokens){target="_blank" rel="noopener noreferrer"}
* [Validate ID tokens](/docs/guides/validate-id-tokens){target="_blank" rel="noopener noreferrer"}
* [Spring Security SAML](/code/java/spring_security_saml/){target="_blank" rel="noopener noreferrer"}

### Related Blog Posts

* [Build a Basic CRUD App with Angular 5.0 and Spring Boot 2.0](/blog/2017/12/04/basic-crud-angular-and-spring-boot){target="_blank" rel="noopener noreferrer"}
* [Use React and Spring Boot to Build a Simple CRUD App](/blog/2018/07/19/simple-crud-react-and-spring-boot){target="_blank" rel="noopener noreferrer"}
* [Secure a Spring Microservices Architecture with Spring Security and OAuth 2.0](/blog/2018/02/13/secure-spring-microservices-with-oauth){target="_blank" rel="noopener noreferrer"}
* [10 Excellent Ways to Secure Your Spring Boot Application](/blog/2018/07/30/10-ways-to-secure-spring-boot){target="_blank" rel="noopener noreferrer"}
* [Build a Basic CRUD App with Angular 7.0 and Spring Boot 2.1](/blog/2018/08/22/basic-crud-angular-7-and-spring-boot-2){target="_blank" rel="noopener noreferrer"}
* [Tutorial: Develop a Mobile App With Ionic and Spring Boot](/blog/2017/05/17/develop-a-mobile-app-with-ionic-and-spring-boot){target="_blank" rel="noopener noreferrer"}
* [Build Your First Progressive Web Application with Angular and Spring Boot](/blog/2017/05/09/progressive-web-applications-with-angular-and-spring-boot){target="_blank" rel="noopener noreferrer"}
* [Secure your SPA with Spring Boot and OAuth](/blog/2017/10/27/secure-spa-spring-boot-oauth){target="_blank" rel="noopener noreferrer"}
* [Add Social Login to Your Spring Boot 2.0 App](/blog/2018/07/24/social-spring-boot){target="_blank" rel="noopener noreferrer"}
* [Secure Your Spring Boot Application with Multi-Factor Authentication](/blog/2018/06/12/mfa-in-spring-boot){target="_blank" rel="noopener noreferrer"}