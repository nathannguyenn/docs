---
title: "Golang SDK Library"
tags: ["Golang", "SDK"]
description: "This is a document detailing the LoginRadius Golang SDK Library."
path: "/references/sdk/golang"
---

# Golang SDK

> **Disclaimer:** This library is meant to help you with a quick implementation of the LoginRadius platform and also to serve as a reference point for the LoginRadius API. Keep in mind that it is an open source library, which means you are free to download and customize the library functions based on your specific application needs.


This document contains information and examples regarding the LoginRadius Golang SDK. It provides guidance for working with social authentication, user profile data, and sending messages with a variety of social networks such as Facebook, Google, Twitter, Yahoo, LinkedIn, and more. You can get the SDK from [here](https://github.com/LoginRadius/go-sdk)

## SDK Installation and Configuration

> Note: Before starting, install Golang and setup the proper paths. Find the instructions [here](https://golang.org/doc/install).

```
   go get github.com/loginradius/go-sdk
```

Import the Golang sdk package on the top of the code that calls the SDK.


```
    import "github.com/loginradius/go-sdk"


    config := lr.Config{
        ApiKey:    {{ Your API KEY }},
        ApiSecret: {{ Your API Secret }},
    }

    lrclient, err := lr.NewLoginradius(&config)

    if err != nil {
        // handle error
    }
```


Replace the placeholders in the config object with your LoginRadius credentials apikey, apisecret, sitename. These can be obtained from [here](https://dashboard.loginradius.com/dashboard). Also, whitelist your application domain as explained in [this document](/tutorial/golang/#whitelist-domain).

Many API calls included in this SDK must be completed with an access token, which can be obtained after calling the Authentication Login API and reading the token from the response or from generating an access token through the UID in the Accounts API.

For APIs that require the user's credentials to function properly, the access token must be passed in the `Authorization: Bearer` header; this is handled by the SDK. For APIs that require the user's access token, initialize the LoginRadius client like so:

```
    lrclient, err := lr.NewLoginradius(&cfg, map[string]string{"token": <access token>})

    if err != nil {
        // handle error
    }
```


Alternatively an already-initalized client can be reused like so:


```
    lrclient.Context.Token = <access token>
```


Please be aware of the dangers of using global variables to store individual user's access token if you choose to reuse an already-initalized client.

## Calling an API

API calls are separated into separate packages. Each package contains a struct holding the LoginRadius client object as an embedded struct, e.g.:

```
    package lraccount

    import (
    	lr "github.com/LoginRadius/go-sdk"
    )

    // Embed Loginradius struct
    type Loginradius struct {
    	Client *lr.Loginradius
    }
```


This allows for the individual API calls to be defined as methods of the Loginradius API client struct.

Require the package containing the API to be called, and type assert the intialized LoginRadius client into the specific package's client struct when calling the API:

```
    res, err := lraccount.Loginradius(lraccount.Loginradius{lrclient}).GetManageAccountProfilesByEmail(map[string]string{"email": "<email>"})
```


## API Methods

### Authentication API

The Authentication (Auth) APIs allow changes to the account once some form of authentication has been performed. For this reason, they are considered to be user facing client-side/front-end API calls.

To call an Authentication API, import the `authentication` package like so:

```
    import (
     github.com/loginradius/go-sdk/api/authentication
    )
```

List of APIs in this Section:

*   PUT : [Auth Change Password](#auth-change-password-put)
*   PUT : [Auth Resend Email Verification](#auth-resend-email-verification-put)
*   POST : [Auth Login by Email](#auth-login-by-email-post)
*   POST : [Auth Forgot Password](#auth-forgot-password-post)
*   POST : [Auth User Registration by Email](#auth-user-registration-by-email-post)
*   GET : [Auth Validate Access token](#auth-validate-access-token-get)
*   GET : [Auth Read all Profiles by Token](#auth-read-all-profiles-by-token-get)
*   GET : [Auth Send Welcome Email](#auth-send-welcome-email-get)
*   GET : [Auth Delete Account](#auth-delete-account-get)
*   GET : [Auth Verify Email](#auth-verify-email-get)
*   GET : [Auth Social Identity](#auth-social-identity-get)
*   DELETE : [Auth Delete Account with Email Confirmation](#auth-delete-account-with-email-confirmation-delete)

#### Auth Change Password (PUT)

This API is used to change the accounts password based on the previous password

```
    res, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).PutAuthChangePassword(
      map[string]string{
        "oldpassword": <oldpassword>,
        "newpassword": <newpassword>,
      }
    )

    if err != nil {
      // handle error
    }
```


#### Auth Resend Email Verification (PUT)

This API resends the verification email to the user.


```
    res, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).PutResendEmailVerification(
      map[string]string{"email": <email>},
    )

    if err != nil {
      //handle error
    }
```


#### Auth Login by Email (POST)

This API retrieves a copy of the user data based on the Email


```
    // use an anonymous struct, alternatively []byte could be passed in lieu of struct
    body := struct {
     Emailstring
     Password string
    }{
     userName,
     email, // uses generated email as password
    }

    res, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient})    .PostAuthLoginByEmail(body)

    if err != nil {
       // handle error
    }
```

#### Auth Forgot Password (POST)

This API is used to send the reset password url to a specified account. Note: If you have the UserName workflow enabled, you may replace the 'email' parameter with 'username'


```
    // use an anonymous struct, alternatively []byte could be passed
    email := struct {
      Email string
    }{
      "example@example.com,
    }

    res, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).
        PostAuthForgotPassword(
          email,
          map[string]string{"resetpasswordurl": "example.com/password/reset"},
        )
    if err != nil {
        // handle error
    }
```


#### Auth User Registration by Email (POST)

This API creates a user in the database as well as sends a verification email to the user.

```
    // Use struct provided by lrbody package to construct body
    // alternatively you could initialize your own struct
    // []byte could also be passed in lieu of a struct
    user := lrbody.RegistrationUser{
       Email: []lrbody.AuthEmail{
         lrbody.AuthEmail{
           Type:  "Primary", //This can be any value of your designation
           Value: "example@example.com",
         },
       },
       Password: "password",
    }

    res, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).
       PostAuthUserRegistrationByEmail(user)

    if err != nil {
       // handle error
    }
```

#### Auth Validate Access token (GET)

This api validates access token, if valid then returns a response with its expiry otherwise error.

```
    res, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).
     GetAuthValidateAccessToken()

    if err != nil {
     //handle error
    }
```


#### Auth Read all Profiles by Token (GET)

This API retrieves a copy of the user data based on the access token.


```
    response, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).
      GetAuthReadProfilesByToken()

    if err != nil {
      // handle error
    }
```


#### Auth Send Welcome Email (GET)

This API sends a welcome email


```
    res, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).GetAuthSendWelcomeEmail(map[string]string{"welcomeemailtemplate": "customer-email-template"})

    if err != nil {
      //handle error
    }
```


#### Auth Delete Account (GET)

This API is used to delete an account by passing it a delete token.


```
    res, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).GetAuthDeleteAccount(
      map[string]string{"deletetoken": "<delete token>"}
    )

    if err != nil {
      // handle error
    }
```


#### Auth Verify Email (GET)

This API is used to verify the email of user. Note: This API will only return the full profile if you have 'Enable auto login after email verification' set in your LoginRadius Dashboard's Email Workflow settings under 'Verification Email'.

```
    res, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).GetAuthVerifyEmail(
      map[string]string{"verificationtoken": <verificationToken>},
    )

    if err != nil {
      //handle error
    }
```

#### Auth Social Identity (GET)

This API is called just after account linking API and it prevents the raas profile of the second account from getting created.

```
    res, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).PutAuthLinkSocialIdentities(
      map[string]string{
        "candidatetoken": <candidatetoken>,
      }
    )

    if err != nil {
      // handle error
    }
```


#### Auth Delete Account with Email Confirmation (DELETE)

This API will send a confirmation email for account deletion to the customer's email when passed the customer's access token

```
    resp, err := lrauthentication.Loginradius(lrauthentication.Loginradius{lrclient}).DeleteAuthDeleteAccountEmailConfirmation()

    if err != nil {
      // handle error
    }
```

### Account API

The Account Management APIs are used to manage a user's account. These calls require the API Key and API Secret and often the User's Account UID (Unified Identifier) to perform an operation. For this reason, these APIs are considered to be for back-end purposes.

To call an Account API, import the `account` package like so:


```
    import (
        lraccount "github.com/LoginRadius/go-sdk/api/account"
    )
```

List of APIs in this Section:

*   PUT : [Account Update](#account-update-put)
*   PUT : [Account Set Password](#account-set-password-put)
*   POST : [Account Create](#account-create-post)
*   POST : [Forgot Password token](#forgot-password-token-post)
*   POST : [Email Verification token](#email-verification-token-post)
*   GET : [Account Profiles by Email](#account-profiles-by-email-get)
*   GET : [Account Profiles by UID](#account-profiles-by-uid-get)
*   GET : [Account Password](#account-password-get)
*   GET : [Account Identities by Email](#account-identities-by-email-get)
*   DELETE : [Account Delete](#account-delete-delete)

#### Account Update (PUT)

This API is used to update the information of existing accounts in your Cloud Storage.


```
    // This example passes a []byte as body
    // Alternatively a struct containing fields to be updated
    // can also be passed in the body
    _, err := lraccount.Loginradius(lraccount.Loginradius{lrclient}).PutManageAccountUpdate(
      uid,
      []byte(`{"Username":"newname"}`), // add profile fields as needed
    )
    if err != nil {
      // handle error
    }
```


#### Account Set Password (PUT)

This API is used to set the password of an account in Cloud Storage.


```
    response, err := lraccount.Loginradius(lraccount.Loginradius{lrclient}).PutManageAccountSetPassword(
     <uid>,
     map[string]string{"password":<new password>},
    )

    if err != nil {
     // handle error
    }
```


#### [](#account-create-post)Account Create (POST)

This API is used to create an account in Cloud Storage. This API bypass the normal email verification process and manually creates the user.


```
    // Use struct provided by lrbody package to construct body
    // alternatively you could initialize your own struct
    // []byte could also be passed in lieu of a struct
    user := lrbody.AccountCreate{
        Email: []lrbody.EmailArray{
          lrbody.EmailArray{
            Type:  "Primary", //This can be any value of your designation
            Value: "example@example.com",
          },
        },
        Password: "password",
        // add more profile fields as needed
    }

    response, err := lraccount.Loginradius(loginradius).PostManageAccountCreate(user)
    if err != nil {
      // handle error
    }
```

#### Forgot Password token (POST)

This API Returns a Forgot Password Token it can also be used to send a Forgot Password email to the customer.

```
    response, err = lraccount.Loginradius(lraccount.Loginradius{loginradius}).PostManageForgotPasswordToken(
      map[string]string{"email":<email>},
      map[string]string{"sendemail": "true"}), //queries are optional
    )

    if err != nil {
      // handle error
    }
```


#### Email Verification token (POST)

This API Returns an Email Verification token.

```
    response, err := lraccount.Loginradius(lraccount.Loginradius{loginradius}).PostManageEmailVerificationToken(
     map[string]string{"email": <email>},
    )

    if err != nil {
     // handle error
    }
```


#### Account Profiles by Email (GET)

This API is used to retrieve all of the profile data, associated with the specified account by email in Cloud Storage.

```
    response, err := lraccount.Loginradius(lraccount.Loginradius{lrclient}).GetManageAccountProfilesByEmail(
      map[string]string{"email": <email>},
    )
    if err != nil {
      // handle error
    }
```


#### Account Profiles by UID (GET)

This API is used to retrieve all of the profile data, associated with the account by uid in Cloud Storage.


```
    response, err := lraccount.Loginradius(lraccount.Loginradius{lrclient}).GetManageAccountProfilesByUid(<uid>)
    if err != nil {
       // handle error
    }
```

#### Account Password (GET)

This API use to retrive the hashed password of a specified account in Cloud Storage.

```
    response, err := lraccount.Loginradius(lraccount.Loginradius{loginradius}).GetManageAccountPassword(uid)
    if err != nil {
      // handle error
    }
```


#### Account Identities by Email (GET)

> Note: This is intended for specific workflows where an email may be associated to multiple UIDs. This API is used to retrieve all of the identities (UID and Profiles), associated with a specified email in Cloud Storage.


```
    response, err := lraccount.Loginradius(lraccount.Loginradius{lrclient}).GetManageAccountProfilesByEmail(
      map[string]string{"email": <email>},
    )
    if err != nil {
      // handle error
    }
```


#### Account Delete (DELETE)

This API deletes the Users account and allows them to re-register for a new account.


```
    res, err := lraccount.Loginradius(lraccount.Loginradius{lrclient}).DeleteManageAccount(<uid>)
    if err != nil {
      // handle error
    }
```


### Social API

The Social APIs are used to fetch user profile and other data from providers linked to the user accounts. The access tokens in this section are obtained after validating an access token using a social provider. Look at Access Token via Facebook, Access Token via Twitter, Access Token via VKontakte to get these access tokens.

To call a Social API, import the `social` package like so:


```
    import (
      lrsocial "github.com/LoginRadius/go-sdk/api/social"
    )
```


List of APIs in this Section:

*   GET : [Access Token](#access-token-get)
*   GET : [Token Validate](#token-validate-get)
*   GET : [Access Token Invalidate](#access-token-invalidate-get)
*   GET : [Get Active Session By Account Id](#get-active-session-by-account-id-get)
*   GET : [User Profile](#user-profile-get)

#### Access Token (GET)

This API Is used to translate the Request Token returned during authentication into an Access Token that can be used with other API calls.


```
    resp, err := lrsocial.Loginradius(lrsocial.Loginradius{lrclient}).GetSocialAccessToken(<request token>)

    if err != nil {
      // handle error
    }
```


#### Token Validate (GET)

This API validates access token, if valid then returns a response with its expiry otherwise error.


```
    resp, err := lrsocial.Loginradius(lrsocial.Loginradius{lrclient}).GetSocialTokenValidate()

    if err != nil {
      // handle error
    }
```


#### Access Token Invalidate (GET)

This api invalidates the active access token or expires an access token validity.

```
    resp, err := lrsocial.Loginradius(lrsocial.Loginradius{lrclient}).GetSocialTokenInvalidate()

    if err != nil {
      // handle error
    }
```


#### Get Active Session By Account Id (GET)

This api is used to get all active sessions by AccountID(UID)

```
    res, err := lrconfiguration.Loginradius(lrconfiguration.Loginradius{lrclient}).GetActiveSessionDetails()

    if err != nil {
      // handle error
    }
```

#### User Profile (GET)

The User Profile API is used to get social profile data from the user's social account after authentication.

```
    resp, err := lrsocial.Loginradius(lrsocial.Loginradius{lrclient}).GetSocialUserProfile()
    if err != nil {
      // handle error
    }
```


### NativeSocial API

The Token Management APIs allow management of access tokens and generation tokens usable by the social APIs.

To call a Token Management API, import the `tokenmanagement` package like so:

```
    import (
        "github.com/LoginRadius/go-sdk/api/tokenmanagement"
    )
```


List of APIs in this Section:

*   GET : [Access Token via Facebook Token](#access-token-via-facebook-token-get)
*   GET : [Access Token via Twitter Token](#access-token-via-twitter-token-get)

#### Access Token via Facebook Token (GET)

The API is used to get LoginRadius access token by sending Facebook's access token. It will be valid for the specific duration of time specified in the response.

```
    res, err := tokenmanagement.Loginradius(tokenmanagement.Loginradius{lrclient}).GetAccessTokenViaFacebook(
      map[string]string{"fb_access_token": <fb access token>},
    )

    if err != nil {
      // handle error
    }
```


#### Access Token via Twitter Token (GET)

The API is used to get LoginRadius access token by sending Twitter's access token. It will be valid for the specific duration of time specified in the response.


```
    res, err := tokenmanagement.Loginradius(tokenmanagement.Loginradius{lrclient}).GetAccessTokenViaTwitter(
      map[string]string{
        "tw_access_token": <twitter access token>,
        "tw_token_secret": <twitter token secret>,
      },
    )

    if err != nil {
      // handle error
    }
```




[Go Back to Home Page](/)
