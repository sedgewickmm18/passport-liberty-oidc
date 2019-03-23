# Passport-OpenID Connect

[Passport](https://github.com/jaredhanson/passport) strategy for authenticating
with [OpenID Connect](http://openid.net/connect/).

This module lets you authenticate using OpenID Connect in your Node.js
applications.  By plugging into Passport, OpenID Connect authentication can be
easily and unobtrusively integrated into any application or framework that
supports [Connect](http://www.senchalabs.org/connect/)-style middleware,
including [Express](http://expressjs.com/).

#### Liberty

Liberty as OIDC provider doesn't expect the schema as query paramater to the userinfo endpoint (and this query parameter doesn't seem to be part of the OIDC spec)
Just specify the isLiberty option to use the plain userinfo endpoint


**Example**

```
passport.use(new Strategy({
    scope: '',   // openid by default
    isLiberty: true,
    issuer: 'https://localhost:9443/oidc/endpoint/markus',
    clientID: 'markus01',
    clientSecret: 'markus01',
    authorizationURL: 'https://localhost:9443/oidc/endpoint/markus/authorize',
    tokenURL: 'https://localhost:9443/oidc/endpoint/markus/token',
    callbackURL: 'https://localhost:3000/callback',
    userInfoURL: 'https://localhost:9443/oidc/endpoint/markus/userinfo'
```

The corresponding setup on liberty's server.xml

```
        <featureManager>
                <feature>openidConnectServer-1.0</feature>
                <feature>servlet-3.1</feature>
                <feature>ssl-1.0</feature>
                <feature>appSecurity-2.0</feature>
                ..

        </featureManager>

        ..

        <!-- grant access to all defined users -->
        <oauth-roles>
           <authenticated>
             <special-subject type="ALL_AUTHENTICATED_USERS" />
           </authenticated>
        </oauth-roles>

        <!-- define the provider -->
        <openidConnectProvider id="markus" oauthProviderRef="markus" />

        <!-- OIDC relies on oauth provider -->
        <!-- OIDC define client-id and client-secret pair and whitelist your callbacks -->
        <oauthProvider id="markus" jwtAccessToken="true">
          <localStore>
            <client name="markus01" secret="markus01" displayname="Markus" scope="openid profile test" enabled="true" preAuthorizedScope="openid profile">
              <redirect>https://localhost:3000/callback</redirect>
             </client>
           </localStore>
        </oauthProvider>
```



## Credits

  - [Jared Hanson](http://github.com/jaredhanson)

## License

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2011-2013 Jared Hanson <[http://jaredhanson.net/](http://jaredhanson.net/)>

<a target='_blank' rel='nofollow' href='https://app.codesponsor.io/link/vK9dyjRnnWsMzzJTQ57fRJpH/jaredhanson/passport-openidconnect'>  <img alt='Sponsor' width='888' height='68' src='https://app.codesponsor.io/embed/vK9dyjRnnWsMzzJTQ57fRJpH/jaredhanson/passport-openidconnect.svg' /></a>
