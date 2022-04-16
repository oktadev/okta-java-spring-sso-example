# Build a Single Sign-on Application in Java
 
This example app demonstrates how to use Okta and Spring Boot to implement single sign-on with a separate client application and resource server.

Please read [Build a Single Sign-on Application in Java](https://developer.okta.com/blog/2020/01/29/java-single-sign-on) to see how this app was created.

**Prerequisites:** 

**Java 11**: This project uses Java 11. If you don't have Java 11, you can install OpenJDK. Instructions are found on the [OpenJDK website](https://openjdk.java.net/install/).

**Okta Developer Account**: You'll be using Okta as an OAuth/OIDC provider to add JWT authentication and authorization to the application. Go to [their website](https://developer.okta.com/signup/) and sign up for one of their free developer accounts, if you haven't already.

> [Okta](https://developer.okta.com/) has Authentication and User Management APIs that reduce development time with instant-on, scalable user infrastructure. Okta's intuitive API and expert support make it easy for developers to authenticate, manage, and secure users and roles in any application.

* [Getting Started](#getting-started)
* [Start the Apps](#start-the-apps)
* [Links](#links)
* [Help](#help)
* [License](#license)

## Getting Started

To install this example application, run the following commands:

```bash
git clone https://github.com/oktadev/okta-java-spring-sso-example.git java-single-sign-on
cd java-single-sign-on
```

This will get a copy of the project installed locally. Before the projects apps will run, however, you need to create an OIDC application in Okta and configure the client and server to use it.

Note: the tutorial linked to at the top of the README demonstrates how to configure two instances of a client application and a resource server running on a custom Okta authorization server. If you would like to see how that is accomplished, please refer to the tutorial. In this README, the instructions demonstrate how to get a single client and the resource server running using the default Okta authorization server. 

### Create an OIDC Server Application in Okta

You will need to create an OIDC Application in Okta to get your values to perform authentication. 

Log in to your Okta Developer account (or [sign up](https://developer.okta.com/signup/) if you don’t have an account) and navigate to **Applications** > **Add Application**. Click **Service**, click **Next**, and give the app a name you’ll remember. You'll need the Client ID and Client Secret for the resource server below.

#### Resource Server Configuration

Open `oauth2-resource-server/src/main/application.properties` and REPLACE the contents with the values below. Set the `issuer`, `clientSecret`, and `clientId` values.

You can find your Issuer URI by going to **API**->**Authorization Servers** and looking next to the `default` server in the table.

```properties
okta.oauth2.issuer={yourIssuerUri}
okta.oauth2.clientId={yourClientID}
okta.oauth2.clientSecret={yourClientSecret}
okta.oauth2.audience=api://default
server.port=8082
```

Notice that here the audience is `api://default` instead of `api://oidcauthserver` that is used for the custom authorization server in the tutorial.

### Create an OIDC Web Client Application in Okta

You will need to create another OIDC Application in Okta to get your values to perform authentication. 

Log in to your Okta Developer account and navigate to **Applications** > **Add Application**. Click **Single-Page App**, click **Next**, and give the app a name you’ll remember. Specify `http://localhost:8080/login/oauth2/code/okta` as a **Login Redirect URI**. Specify `http://localhost:8080` as a **Base URI**. Click **Done**. 

You'll use the ClientID and Client Secret below to configure the client application.

#### Client Configuration

Open `oauth2-client/src/main/application.properties` and REPLACE the contents with the contents below. Set the `issuer`, `clientSecret`, and `clientId` values.

```properties
okta.oauth2.issuer={yourIssuerUri}
okta.oauth2.clientId={yourClientID}
okta.oauth2.clientSecret={yourClientSecret}
okta.oauth2.scopes=openid,profile
server.port=8080
resourceServer.url=http://localhost:8082
```

## Start the Apps

To install all of its dependencies and start each app, follow the instructions below.

To run the server, from a shell, open the `oauth2-resource-server` directory and run:
 
```bash
./mvnw spring-boot:run
```

To run the client, from a different shell, open the `oauth2-client` directory and run:
 
```bash
./mvnw spring-boot:run
```

You can now test the client application by opening http://localhost:8080

## Links

This example uses the following open source libraries:

* [Spring Boot](https://spring.io/projects/spring-boot)
* [Spring Security](https://spring.io/projects/spring-security)

## Help

Please post any questions as comments on the [blog post](https://developer.okta.com/blog/2020/01/29/java-single-sign-on), or visit our [Okta Developer Forums](https://devforum.okta.com/).

## License

Apache 2.0, see [LICENSE](LICENSE).
