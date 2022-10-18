# Artemis Configuration SAML
Test environment for the SAML2/Shibboleth Feature in [Artemis](https://github.com/ls1intum/Artemis).

*Instead of configuring the whole containers you can obtain a preconfigured [data archive](https://github.com/kit-sdq/Artemis-SAML2-Test-Docker/releases/download/v5.1.3/data.tar.gz) from the [releases page](https://github.com/kit-sdq/Artemis-SAML2-Test-Docker/releases) or iff not present by writing an email to "dominik (dot) fuchss (at) kit (dot) edu"*


## Some interesting effects by OS

### Mac OS
It seems that gitlab is not able to start on MacOS if you use file system mapped volumes.
So you may have to copy the contents for gitlab to a "real" volume

### Linux Users (especially WSL users)
You may have to ..
* use `find . -type f -print0 | xargs -0 dos2unix` to convert line endings to linux style (especially the docker folder)
* use `chmod 777 -R data` to fix any issues regarding the permissions of the data archive (data.7z) (only iff you use the archive)

## Instructions for local testing

1. Clone the repository
2. Run `docker compose up -d` and wait for the initial startup of the backend (config files will be copied). After that, stop the backend: `docker-compose stop artemis`
3. Configure Artemis with Jenkins and Gilab as usual. Setup SAML using the configuration file as follows:

The application-saml2.yml must look like that:
```yaml
saml2:
    username-pattern: 'saml2-{first_name}_{last_name}'
    first-name-pattern: '{first_name}'
    last-name-pattern: '{last_name}'
    email-pattern: '{email}'
    registration-number-pattern: '{uid}'
    identity-providers:
        - metadata: http://saml:8080/simplesaml/saml2/idp/metadata.php
          registration-id: testidp
          entity-id: artemis
          cert-file: # data/saml/cert (optional) Set this path to the Certificate for encryption/signing or leave it blank
          key-file: # data/saml/key path-to-key (optional) Set this path to the Key for encryption/ssigning or leave it blank

info.saml2.button-label: 'SAML Login'
info.saml2.enable-password: true
```

4. (Enable Debug output)
5. Start the stack using ` docker-compose up -d`
6. Log in with SAML2 using `user1` and `user1pass`
7. User mail addresses are fixed, testing is in this configuration only possible when overwriting the user information in the SAML docker. 
