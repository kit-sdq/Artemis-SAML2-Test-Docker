# Artemis Configuration SAML
Test environment for the SAML2/Shibboleth Feature in [Artemis](https://github.com/ls1intum/Artemis).

*Instead of configuring the whole containers you can obtain a preconfigured data archive from the [releases page](https://github.com/kit-sdq/Artemis-SAML2-Test-Docker/releases)*


## Some interesting effects by OS

### Linux Users
You may have to ..
* run all commands as root esp. `tar xzf data.tgz` to ensure that permissions are correct

## Instructions for local testing

1. Clone the repository
2. Run `docker compose up -d` and wait for the initial startup of the backend (config files will be copied). After that, stop the backend: `docker-compose stop artemis`
3. Configure Artemis with Jenkins as usual. Setup SAML using the configuration file as follows:

The application-saml2.yml must look like that:
```yaml
spring:
  security:
    saml2:
      relyingparty:
        registration:
          testidp:
            assertingparty:
              metadata-uri: http://saml:8080/simplesaml/saml2/idp/metadata.php

saml2:
    username-pattern: 'saml2-{first_name}_{last_name}'
    first-name-pattern: '{first_name}'
    last-name-pattern: '{last_name}'
    email-pattern: '{email}'
    registration-number-pattern: '{uid}'

# String used for the SAML2 login button. E.g. 'Shibboleth Login'
info.saml2.button-label: 'SAML2 Login'
# Sends a e-mail to the new user with a link to set the Artemis password. This password allows login to Artemis and its
# services such as GitLab and Jenkins. This allows the users to use password-based Git workflows.
# Enabled the password reset function in Artemis.
info.saml2.enable-password: true
```

4. (Enable Debug output)
5. Start the stack using `docker compose up -d`
6. Log in with SAML2 using `user1` and `user1pass`
7. User mail addresses are fixed, testing is in this configuration only possible when overwriting the user information in the SAML docker.
