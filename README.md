# Auth0 Mock API
A simple mock api for [Auth0](https://auth0.com/) to enable offline local development. To install, run: 

    npm -g install api-mock
    git clone https://github.com/neam/auth0-mock-api.git auth0-mock-api
    cd auth0-mock-api
    openssl genrsa -des3 -out server.key 1024 # any passphrase will do
    openssl req -new -key server.key -out server.csr
    openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
    
To start the local mock server:

    api-mock auth0-mock-api-blueprint.md --port 2999 --ssl-port 3000 --ssl-enable --cors-disable false

The local api is then available on https://127.0.0.1:3000/

Note: First time, you need to visit [https://127.0.0.1:3000/tokeninfo](https://127.0.0.1:3000/tokeninfo) an accept the self-signed certificate in each browser that you will be using the mock api.

To use this with auth0-angular, use the following settings in `.config()`:

    authProvider.init({
        domain: '127.0.0.1:3000',
        clientID: 'auth0mockapiclientid',
        ...
    });
    window.auth0mockdata = {
        profile: {"name":"John Doe","given_name":"John","family_name":"Doe","gender":"male","picture":"http://placehold.it/150x150","age_range":{"min":21},"devices":[{"hardware":"iPhone","os":"iOS"}],"updated_time":"2015-09-07T08:55:46+0000","installed":true,"is_verified":false,"locale":"en_US","name_format":"{first} {last}","verified":true,"nickname":"auth0mockapi","user_metadata":{"api_endpoints":[{"slug":"local"}],"default_api_endpoint_slug":"local","original_mixpanel_distinct_id":"mock-original_mixpanel_distinct_id","signup_tracked":true},"email":"john.doe@example.com","app_metadata":{"r0":{"permissions":{"example":{"superuser":0,"groups":[]}}}},"email_verified":true,"clientID":"foobar","updated_at":"2015-09-09T07:25:39.429Z","user_id":"auth0mockapi|123123123123123123","identities":[{"access_token":"foobar","provider":"auth0mockapi","user_id":"123123123123123123","connection":"auth0mockapi","isSocial":true}],"created_at":"2015-09-03T10:10:43.432Z","global_client_id":"foobar"},
        idToken: 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiSm9obiBEb2UiLCJnaXZlbl9uYW1lIjoiSm9obiIsImZhbWlseV9uYW1lIjoiRG9lIiwiZ2VuZGVyIjoibWFsZSIsInBpY3R1cmUiOiJodHRwOi8vcGxhY2Vob2xkLml0LzE1MHgxNTAiLCJhZ2VfcmFuZ2UiOnsibWluIjoyMX0sImRldmljZXMiOlt7ImhhcmR3YXJlIjoiaVBob25lIiwib3MiOiJpT1MifV0sInVwZGF0ZWRfdGltZSI6IjIwMTUtMDktMDdUMDg6NTU6NDYrMDAwMCIsImluc3RhbGxlZCI6dHJ1ZSwiaXNfdmVyaWZpZWQiOmZhbHNlLCJsb2NhbGUiOiJlbl9VUyIsIm5hbWVfZm9ybWF0Ijoie2ZpcnN0fSB7bGFzdH0iLCJ2ZXJpZmllZCI6dHJ1ZSwibmlja25hbWUiOiJhdXRoMG1vY2thcGkiLCJ1c2VyX21ldGFkYXRhIjp7ImFwaV9lbmRwb2ludHMiOlt7InNsdWciOiJsb2NhbCJ9XSwiZGVmYXVsdF9hcGlfZW5kcG9pbnRfc2x1ZyI6ImxvY2FsIiwib3JpZ2luYWxfbWl4cGFuZWxfZGlzdGluY3RfaWQiOiJtb2NrLW9yaWdpbmFsX21peHBhbmVsX2Rpc3RpbmN0X2lkIiwic2lnbnVwX3RyYWNrZWQiOnRydWV9LCJlbWFpbCI6ImpvaG4uZG9lQGV4YW1wbGUuY29tIiwiYXBwX21ldGFkYXRhIjp7InIwIjp7InBlcm1pc3Npb25zIjp7ImV4YW1wbGUiOnsic3VwZXJ1c2VyIjowLCJncm91cHMiOltdfX19fSwiZW1haWxfdmVyaWZpZWQiOnRydWUsImNsaWVudElEIjoiZm9vYmFyIiwidXBkYXRlZF9hdCI6IjIwMTUtMDktMDlUMDc6MjU6MzkuNDI5WiIsInVzZXJfaWQiOiJhdXRoMG1vY2thcGl8MTIzMTIzMTIzMTIzMTIzMTIzIiwiaWRlbnRpdGllcyI6W3siYWNjZXNzX3Rva2VuIjoiZm9vYmFyIiwicHJvdmlkZXIiOiJhdXRoMG1vY2thcGkiLCJ1c2VyX2lkIjoiMTIzMTIzMTIzMTIzMTIzMTIzIiwiY29ubmVjdGlvbiI6ImF1dGgwbW9ja2FwaSIsImlzU29jaWFsIjp0cnVlfV0sImNyZWF0ZWRfYXQiOiIyMDE1LTA5LTAzVDEwOjEwOjQzLjQzMloiLCJnbG9iYWxfY2xpZW50X2lkIjoiZm9vYmFyIn0._HkQuZs6Y6N38biAnksnWg3Ayf3qnE2hwBkeMKfxbiE'
    };

Then in `auth0-angular.js`, replace `signinCall(options);` with the following:

    // Auto-sign-in with fake profile data
    successFn(window.auth0mockdata.profile, window.auth0mockdata.idToken, 'accessToken-foo', 'state-foo');
    //signinCall(options);

All signins will then be successful without any actual external signin process taking place.

Mock API documentation is available on [http://docs.auth0mockapi.apiary.io/](http://docs.auth0mockapi.apiary.io/)

Happy offline dev :)