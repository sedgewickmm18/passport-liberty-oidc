This example demonstrates how to use [Express](http://expressjs.com/) 4.x and
[Passport](http://passportjs.org/) to authenticate users via OpenID Connect.
Use this example as a starting point for your own web applications.

## Instructions

To install this example on your computer, clone the repository and install
dependencies.

This example depends on a liberty based openid connect provider running on localhost.

```bash
$ npm install
$ npm start
```


### Testing with a Jazz Authorization Server

After installing JAS I added the admin center to the list of features in the server.xml (usually in `./wlp/usr/servers/jazzop/server.xml`):

```
    ~        <featureManager>
                <feature>openidConnectServer-1.0</feature>
                <feature>jdbc-4.0</feature>
                <feature>adminCenter-1.0</feature>
``

and granted ADMIN liberty admin rights with the ability to change the configuration from the admin center

```    <administrator-role>
        <user>ADMIN</user>
        <group>JazzAdmins</group>
    </administrator-role>

    <remoteFileAccess>
        <writeDir>${server.config.dir}</writeDir>
    </remoteFileAccess>
```

In `./wlp/usr/servers/jazzop/appConfig.xml` I added the example client to the oauthProvider - see the localStore xml node.

```
        <oauthProvider id="JazzOP"
            httpsRequired="true" 
                autoAuthorize="true"
                customLoginURL="/jazzop/form/login" 
                accessTokenLifetime="7201" 
                authorizationGrantLifetime="604801">
                <autoAuthorizeClient>client01</autoAuthorizeClient>
                <databaseStore dataSourceRef="OAuthFvtDataSource" />
        <localStore>
            <client name="markus01" secret="markus01" displayname="Markus" scope="openid profile test" enabled="true" preAuthorizedScope="openid profile">
                <redirect>https://localhost:3000/callback</redirect>
              </client>
         </localStore>
        </oauthProvider>
```
