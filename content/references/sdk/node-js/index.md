---
title: "Node.js SDK"
tags: ["Node.js", "Node", "JavaScript", "JS", "SDK"]
description: "A guide to impliment LoginRadius SDK in a Node.js application."
path: "/references/sdk/node-js"
---

# Node.js SDK

>**Disclaimer:** This library is meant to help you with a quick implementation of the LoginRadius platform and also to serve as a reference point for the LoginRadius API. Keep in mind that it is an open source library, which means you are free to download and customize the library functions based on your specific application needs.


This document describes how to start using the LoginRadius Node.js SDK and its various features. You can get the SDK from [here](http://github.com/LoginRadius/node-js-sdk).

> Note: The latest version(10.0.0) of Node js SDK works with LoginRadius V2 APIs.

For a more hands-on tutorial on setting up the SDK and using it in a Node.js application to make LoginRadius API calls, check out our [Node.js Tutorial.](/tutorial/node-js)

## SDK Installation and Configuration <br/>

```
npm install loginradius-sdk
```

In your project, create `var config` file:

```
  var config = {
      apiDomain: 'https://api.loginradius.com',
      apiKey: '{{ Your API KEY }}',
      apiSecret: '{{ Your API Secret }}',
      siteName: '{{ Your Sitename }}',
      proxy:{
        host:'',
        port:'',
        user:'',
        password:''
     }
  }
  ```
Replace the placeholders in the config object with your LoginRadius credentials apikey, apisecret, sitename. These can be obtained from [here](https://dashboard.loginradius.com/configuration).

Pass the proxy configurations if you want to set Http Server Proxy Configuration through your Node.js SDK. Host and port are required to set Http Server Proxy configuration (username and password are optional).

Require the loginradius-sdk package and pass the config object:

```PHP
  var lrv2 = require('loginradius-sdk')(config);
 ```

For more details check [API Refrence Here](/#api).

## API Methods

### Authentication API

List of APIs in this Section:
 
* PUT : [Auth Update Profile by Token](#auth-update-profile-by-token-put) 
* PUT : [Auth Verify Email By OTP](#auth-verify-email-by-otp-put) 
* PUT : [Auth Reset Password by Reset Token](#auth-reset-password-by-reset-token-put) 
* PUT : [Auth Change Password](#auth-change-password-put) 
* PUT : [Auth Resend Email Verification](#auth-resend-email-verification-put) 
* POST : [Auth Add Email](#auth-add-email-post) 
* POST : [Auth Login by Email](#auth-login-by-email-post) 
* POST : [Auth Forgot Password](#auth-forgot-password-post) 
* POST : [Auth User Registration by Email](#auth-user-registration-by-email-post) 
* POST : [Auth User Registration By Captcha](#auth-user-registration-by-captcha-post) 
* GET : [Auth Validate Access token](#auth-validate-access-token-get) 
* GET : [Access Token Invalidate](#access-token-invalidate-get) 
* GET : [Access Token Info](#access-token-info-get) 
* GET : [Auth Read all Profiles by Token](#auth-read-all-profiles-by-token-get) 
* GET : [Auth Send Welcome Email](#auth-send-welcome-email-get) 
* GET : [Auth Delete Account](#auth-delete-account-get) 
* GET : [Auth Check Email Availability](#auth-check-email-availability-get) 
* GET : [Auth Verify Email](#auth-verify-email-get) 
* GET : [Auth Social Identity](#auth-social-identity-get) 
* GET : [Auth Privacy Policy Accept](#auth-privacy-policy-accept-get) 
* DELETE : [Auth Delete Account with Email Confirmation](#auth-delete-account-with-email-confirmation-delete) 
* DELETE : [Auth Remove Email](#auth-remove-email-delete)



#### Auth Update Profile by Token (PUT)

This API is used to update the user's profile by passing the access token.

 
 

 ```js

var accessToken = "<accessToken>"; //Required

var userProfileUpdateModel ={ 
"firstName" : "<firstName>",
"lastName" : "<lastName>"
};  //Required
var emailTemplate = "<emailTemplate>"; //Optional
var fields = null; //Optional
var nullSupport = true; //Optional
var smsTemplate = "<smsTemplate>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional

lrv2.authenticationApi.updateProfileByAccessToken(accessToken, userProfileUpdateModel, emailTemplate, fields, nullSupport, smsTemplate, verificationUrl).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Verify Email By OTP (PUT)

This API is used to verify the email of user when the OTP Email verification flow is enabled, please note that you must contact LoginRadius to have this feature enabled.

 
 

 ```js


var emailVerificationByOtpModel ={ 
"email" : "<email>",
"otp" : "<otp>"
};  //Required
var fields = null; //Optional
var url = "<url>"; //Optional
var welcomeEmailTemplate = "<welcomeEmailTemplate>"; //Optional

lrv2.authenticationApi.verifyEmailByOTP(emailVerificationByOtpModel, fields, url, welcomeEmailTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Reset Password by Reset Token (PUT)

This API is used to set a new password for the specified account.

 
 

 ```js


var resetPasswordByResetTokenModel ={ 
"password" : "<password>",
"resetToken" : "<resetToken>"
};  //Required

lrv2.authenticationApi.resetPasswordByResetToken(resetPasswordByResetTokenModel).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Change Password (PUT)

This API is used to change the accounts password based on the previous password

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var newPassword = "<newPassword>"; //Required
var oldPassword = "<oldPassword>"; //Required

lrv2.authenticationApi.changePassword(accessToken, newPassword, oldPassword).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Resend Email Verification (PUT)

This API resends the verification email to the user.

 
 

 ```js

var email = "<email>"; //Required
var emailTemplate = "<emailTemplate>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional

lrv2.authenticationApi.authResendEmailVerification(email, emailTemplate, verificationUrl).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Add Email (POST)

This API is used to add additional emails to a user's account.

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var email = "<email>"; //Required
var type = "<type>"; //Required
var emailTemplate = "<emailTemplate>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional

lrv2.authenticationApi.addEmail(accessToken, email, type, emailTemplate, verificationUrl).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Login by Email (POST)

This API retrieves a copy of the user data based on the Email

 
 

 ```js


var emailAuthenticationModel ={ 
"email" : "<email>",
"password" : "<password>"
};  //Required
var emailTemplate = "<emailTemplate>"; //Optional
var fields = null; //Optional
var loginUrl = "<loginUrl>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional

lrv2.authenticationApi.loginByEmail(emailAuthenticationModel, emailTemplate, fields, loginUrl, verificationUrl).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Forgot Password (POST)

This API is used to send the reset password url to a specified account. Note: If you have the UserName workflow enabled, you may replace the 'email' parameter with 'username'

 
 

 ```js

var email = "<email>"; //Required
var resetPasswordUrl = "<resetPasswordUrl>"; //Required
var emailTemplate = "<emailTemplate>"; //Optional

lrv2.authenticationApi.forgotPassword(email, resetPasswordUrl, emailTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth User Registration by Email (POST)

This API creates a user in the database as well as sends a verification email to the user.

 
 

 ```js


var authUserRegistrationModel ={ 
"email" : [   { 
 "type" : "<type>"  ,
 "value" : "<value>"   
}  ] ,
"firstName" : "<firstName>",
"lastName" : "<lastName>",
"password" : "<password>"
};  //Required
var sott = "<sott>"; //Required
var emailTemplate = "<emailTemplate>"; //Optional
var fields = null; //Optional
var options = "<options>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional
var welcomeEmailTemplate = "<welcomeEmailTemplate>"; //Optional

lrv2.authenticationApi.userRegistrationByEmail(authUserRegistrationModel, sott, emailTemplate, fields, options, verificationUrl, welcomeEmailTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth User Registration By Captcha (POST)

This API creates a user in the database as well as sends a verification email to the user.

 
 

 ```js


var authUserRegistrationModelWithCaptcha ={ 
"email" : [   { 
 "type" : "<type>"  ,
 "value" : "<value>"   
}  ] ,
"firstName" : "<firstName>",
"g-recaptcha-response" : "<g-recaptcha-response>",
"lastName" : "<lastName>",
"password" : "<password>"
};  //Required
var emailTemplate = "<emailTemplate>"; //Optional
var fields = null; //Optional
var options = "<options>"; //Optional
var smsTemplate = "<smsTemplate>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional
var welcomeEmailTemplate = "<welcomeEmailTemplate>"; //Optional

lrv2.authenticationApi.userRegistrationByCaptcha(authUserRegistrationModelWithCaptcha, emailTemplate, fields, options, smsTemplate, verificationUrl, welcomeEmailTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Validate Access token (GET)

This api validates access token, if valid then returns a response with its expiry otherwise error.

 
 

 ```js

var accessToken = "<accessToken>"; //Required

lrv2.authenticationApi.authValidateAccessToken(accessToken).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Access Token Invalidate (GET)

This api call invalidates the active access token or expires an access token's validity.

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var preventRefresh = true; //Optional

lrv2.authenticationApi.authInValidateAccessToken(accessToken, preventRefresh).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Access Token Info (GET)

This api call provide the active access token Information

 
 

 ```js

var accessToken = "<accessToken>"; //Required

lrv2.authenticationApi.getAccessTokenInfo(accessToken).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Read all Profiles by Token (GET)
This API retrieves a copy of the user data based on the access token.

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var fields = null; //Optional

lrv2.authenticationApi.getProfileByAccessToken(accessToken, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Send Welcome Email (GET)

This API sends a welcome email.

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var welcomeEmailTemplate = "<welcomeEmailTemplate>"; //Optional

lrv2.authenticationApi.sendWelcomeEmail(accessToken, welcomeEmailTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Delete Account (GET)

This API is used to delete an account by passing it a delete token.

 
 

 ```js

var deletetoken = "<deletetoken>"; //Required

lrv2.authenticationApi.deleteAccountByDeleteToken(deletetoken).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Check Email Availability (GET)

This API is used to check the email exists or not on your site.

 
 

 ```js

var email = "<email>"; //Required

lrv2.authenticationApi.checkEmailAvailability(email).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Verify Email (GET)

This API is used to verify the email of user. Note: This API will only return the full profile if you have 'Enable auto login after email verification' set in your LoginRadius Admin Console's Email Workflow settings under 'Verification Email'.

 
 

 ```js

var verificationToken = "<verificationToken>"; //Required
var fields = null; //Optional
var url = "<url>"; //Optional
var welcomeEmailTemplate = "<welcomeEmailTemplate>"; //Optional

lrv2.authenticationApi.verifyEmail(verificationToken, fields, url, welcomeEmailTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Social Identity (GET)

This API is called just after account linking API and it prevents the raas profile of the second account from getting created.

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var fields = null; //Optional

lrv2.authenticationApi.getSocialIdentity(accessToken, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Privacy Policy Accept (GET)

This API is used to update the privacy policy stored in the user's profile by providing the access token of the user accepting the privacy policy

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var fields = null; //Optional

lrv2.authenticationApi.acceptPrivacyPolicy(accessToken, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Delete Account with Email Confirmation (DELETE)

This API will send a confirmation email for account deletion to the customer's email when passed the customer's access token

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var deleteUrl = "<deleteUrl>"; //Optional
var emailTemplate = "<emailTemplate>"; //Optional

lrv2.authenticationApi.deleteAccountWithEmailConfirmation(accessToken, deleteUrl, emailTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Auth Remove Email (DELETE)

This API is used to remove additional emails from a user's account.

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var email = "<email>"; //Required

lrv2.authenticationApi.removeEmail(accessToken, email).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  

 

### Account API


List of APIs in this Section:
 
* PUT : [Account Update](#account-update-put) 
* PUT : [Update Phone ID by UID](#update-phone-id-by-uid-put) 
* PUT : [Account Set Password](#account-set-password-put) 
* PUT : [Account Invalidate Verification Email](#account-invalidate-verification-email-put) 
* PUT : [Reset phone ID verification](#reset-phone-id-verification-put) 
* PUT : [Upsert Email](#upsert-email-put) 
* PUT : [Update UID](#update-uid-put) 
* POST : [Account Create](#account-create-post) 
* POST : [Forgot Password token](#forgot-password-token-post) 
* POST : [Email Verification token](#email-verification-token-post) 
* GET : [Account Profiles by Email](#account-profiles-by-email-get) 
* GET : [Account Profile by Phone ID](#account-profile-by-phone-id-get) 
* GET : [Account Profiles by UID](#account-profiles-by-uid-get) 
* GET : [Account Password](#account-password-get) 
* GET : [Access Token based on UID or User impersonation API](#access-token-based-on-uid-or-user-impersonation-api-get) 
* GET : [Account Identities by Email](#account-identities-by-email-get) 
* DELETE : [Account Delete](#account-delete-delete) 
* DELETE : [Account Remove Email](#account-remove-email-delete) 
* DELETE : [Delete User Profiles By Email](#delete-user-profiles-by-email-delete)



#### Account Update (PUT)

This API is used to update the information of existing accounts in your Cloud Storage. See our Advanced API Usage section <a href='https://www.loginradius.com/docs/api/v2/customer-identity-api/advanced-api-usage/'>Here</a> for more capabilities.

 
 

 ```js


var accountUserProfileUpdateModel ={ 
"firstName" : "<firstName>",
"lastName" : "<lastName>"
};  //Required
var uid = "<uid>"; //Required
var fields = null; //Optional
var nullSupport = true; //Optional

lrv2.accountApi.updateAccountByUid(accountUserProfileUpdateModel, uid, fields, nullSupport).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Update Phone ID by UID (PUT)

This API is used to update the PhoneId by using the Uid's. Admin can update the PhoneId's for both the verified and unverified profiles. It will directly replace the PhoneId and bypass the OTP verification process.

 
 

 ```js

var phone = "<phone>"; //Required
var uid = "<uid>"; //Required
var fields = null; //Optional

lrv2.accountApi.updatePhoneIDByUid(phone, uid, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Set Password (PUT)

This API is used to set the password of an account in Cloud Storage.

 
 

 ```js

var password = "<password>"; //Required
var uid = "<uid>"; //Required

lrv2.accountApi.setAccountPasswordByUid(password, uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Invalidate Verification Email (PUT)

This API is used to invalidate the Email Verification status on an account.

 
 

 ```js

var uid = "<uid>"; //Required
var emailTemplate = "<emailTemplate>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional

lrv2.accountApi.invalidateAccountEmailVerification(uid, emailTemplate, verificationUrl).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Reset phone ID verification (PUT)

This API Allows you to reset the phone no verification of an end user’s account.

 
 

 ```js

var uid = "<uid>"; //Required
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.accountApi.resetPhoneIDVerificationByUid(uid, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Upsert Email (PUT)

This API is used to add/upsert another emails in account profile by different-different email types. If the email type is same then it will simply update the existing email, otherwise it will add a new email in Email array.

 
 

 ```js


var upsertEmailModel ={ 
"email" : [   { 
 "type" : "<type>"  ,
 "value" : "<value>"   
}  ] 
};  //Required
var uid = "<uid>"; //Required
var fields = null; //Optional

lrv2.accountApi.upsertEmail(upsertEmailModel, uid, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Update UID (PUT)

This API is used to update a user's Uid. It will update all profiles, custom objects and consent management logs associated with the Uid.

 
 

 ```js


var updateUidModel ={ 
"newUid" : "<newUid>"
};  //Required
var uid = "<uid>"; //Required

lrv2.accountApi.accountUpdateUid(updateUidModel, uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Create (POST)

This API is used to create an account in Cloud Storage. This API bypass the normal email verification process and manually creates the user. 
 
 In order to use this API, you need to format a JSON request body with all of the mandatory fields

 
 

 ```js


var accountCreateModel ={ 
"email" : [   { 
 "type" : "<type>"  ,
 "value" : "<value>"   
}  ] ,
"firstName" : "<firstName>",
"lastName" : "<lastName>",
"password" : "<password>"
};  //Required
var fields = null; //Optional

lrv2.accountApi.createAccount(accountCreateModel, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Forgot Password token (POST)

This API Returns a Forgot Password Token it can also be used to send a Forgot Password email to the customer. Note: If you have the UserName workflow enabled, you may replace the 'email' parameter with 'username' in the body.

 
 

 ```js

var email = "<email>"; //Required
var emailTemplate = "<emailTemplate>"; //Optional
var resetPasswordUrl = "<resetPasswordUrl>"; //Optional
var sendEmail = true; //Optional

lrv2.accountApi.getForgotPasswordToken(email, emailTemplate, resetPasswordUrl, sendEmail).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Email Verification token (POST)

This API Returns an Email Verification token.

 
 

 ```js

var email = "<email>"; //Required

lrv2.accountApi.getEmailVerificationToken(email).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Profiles by Email (GET)

This API is used to retrieve all of the profile data, associated with the specified account by email in Cloud Storage.

 
 

 ```js

var email = "<email>"; //Required
var fields = null; //Optional

lrv2.accountApi.getAccountProfileByEmail(email, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Profile by Phone ID (GET)

This API is used to retrieve all of the profile data, associated with the account by phone number in Cloud Storage.

 
 

 ```js

var phone = "<phone>"; //Required
var fields = null; //Optional

lrv2.accountApi.getAccountProfileByPhone(phone, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Profiles by UID (GET)

This API is used to retrieve all of the profile data, associated with the account by uid in Cloud Storage.

 
 

 ```js

var uid = "<uid>"; //Required
var fields = null; //Optional

lrv2.accountApi.getAccountProfileByUid(uid, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Password (GET)

This API use to retrive the hashed password of a specified account in Cloud Storage.

 
 

 ```js

var uid = "<uid>"; //Required

lrv2.accountApi.getAccountPasswordHashByUid(uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Access Token based on UID or User impersonation API (GET)

The API is used to get LoginRadius access token based on UID.

 
 

 ```js

var uid = "<uid>"; //Required

lrv2.accountApi.getAccessTokenByUid(uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Identities by Email (GET)

Note: This is intended for specific workflows where an email may be associated to multiple UIDs. This API is used to retrieve all of the identities (UID and Profiles), associated with a specified email in Cloud Storage.

 
 

 ```js

var email = "<email>"; //Required
var fields = null; //Optional

lrv2.accountApi.getAccountIdentitiesByEmail(email, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Delete (DELETE)

This API deletes the Users account and allows them to re-register for a new account.

 
 

 ```js

var uid = "<uid>"; //Required

lrv2.accountApi.deleteAccountByUid(uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Remove Email (DELETE)

Use this API to Remove emails from a user Account

 
 

 ```js

var email = "<email>"; //Required
var uid = "<uid>"; //Required
var fields = null; //Optional

lrv2.accountApi.removeEmail(email, uid, fields).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Delete User Profiles By Email (DELETE)

This API is used to delete all user profiles associated with an Email.

 
 

 ```js

var email = "<email>"; //Required

lrv2.accountApi.accountDeleteByEmail(email).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
 

### PhoneAuthentication API


List of APIs in this Section:
 
* PUT : [Phone Reset Password by OTP](#phone-reset-password-by-otp-put) 
* PUT : [Phone Verification OTP](#phone-verification-otp-put) 
* PUT : [Phone Verification OTP by Token](#phone-verification-otp-by-token-put) 
* PUT : [Phone Number Update](#phone-number-update-put) 
* POST : [Phone Login](#phone-login-post) 
* POST : [Phone Forgot Password by OTP](#phone-forgot-password-by-otp-post) 
* POST : [Phone Resend Verification OTP](#phone-resend-verification-otp-post) 
* POST : [Phone Resend Verification OTP By Token](#phone-resend-verification-otp-by-token-post) 
* POST : [Phone User Registration by SMS](#phone-user-registration-by-sms-post) 
* GET : [Phone Number Availability](#phone-number-availability-get) 
* DELETE : [Remove Phone ID by Access Token](#remove-phone-id-by-access-token-delete)



#### Phone Reset Password by OTP (PUT)

This API is used to reset the password.

 
 

 ```js


var resetPasswordByOTPModel ={ 
"otp" : "<otp>",
"password" : "<password>",
"phone" : "<phone>"
};  //Required

lrv2.phoneAuthenticationApi.resetPasswordByPhoneOTP(resetPasswordByOTPModel).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Phone Verification OTP (PUT)

This API is used to validate the verification code sent to verify a user's phone number

 
 

 ```js

var otp = "<otp>"; //Required
var phone = "<phone>"; //Required
var fields = null; //Optional
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.phoneAuthenticationApi.phoneVerificationByOTP(otp, phone, fields, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Phone Verification OTP by Token (PUT)

This API is used to consume the verification code sent to verify a user's phone number. Use this call for front-end purposes in cases where the user is already logged in by passing the user's access token.

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var otp = "<otp>"; //Required
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.phoneAuthenticationApi.phoneVerificationOTPByAccessToken(accessToken, otp, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Phone Number Update (PUT)

This API is used to update the login Phone Number of users

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var phone = "<phone>"; //Required
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.phoneAuthenticationApi.updatePhoneNumber(accessToken, phone, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Phone Login (POST)

This API retrieves a copy of the user data based on the Phone

 
 

 ```js


var phoneAuthenticationModel ={ 
"password" : "<password>",
"phone" : "<phone>"
};  //Required
var fields = null; //Optional
var loginUrl = "<loginUrl>"; //Optional
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.phoneAuthenticationApi.loginByPhone(phoneAuthenticationModel, fields, loginUrl, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Phone Forgot Password by OTP (POST)

This API is used to send the OTP to reset the account password.

 
 

 ```js

var phone = "<phone>"; //Required
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.phoneAuthenticationApi.forgotPasswordByPhoneOTP(phone, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Phone Resend Verification OTP (POST)

This API is used to resend a verification OTP to verify a user's Phone Number. The user will receive a verification code that they will need to input

 
 

 ```js

var phone = "<phone>"; //Required
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.phoneAuthenticationApi.phoneResendVerificationOTP(phone, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Phone Resend Verification OTP By Token (POST)

This API is used to resend a verification OTP to verify a user's Phone Number in cases in which an active token already exists

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var phone = "<phone>"; //Required
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.phoneAuthenticationApi.phoneResendVerificationOTPByToken(accessToken, phone, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Phone User Registration by SMS (POST)

This API registers the new users into your Cloud Storage and triggers the phone verification process.

 
 

 ```js


var authUserRegistrationModel ={ 
"firstName" : "<firstName>",
"lastName" : "<lastName>",
"password" : "<password>",
"phoneId" : "<phoneId>"
};  //Required
var sott = "<sott>"; //Required
var fields = null; //Optional
var options = "<options>"; //Optional
var smsTemplate = "<smsTemplate>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional
var welcomeEmailTemplate = "<welcomeEmailTemplate>"; //Optional

lrv2.phoneAuthenticationApi.userRegistrationByPhone(authUserRegistrationModel, sott, fields, options, smsTemplate, verificationUrl, welcomeEmailTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Phone Number Availability (GET)

This API is used to check the Phone Number exists or not on your site.

 
 

 ```js

var phone = "<phone>"; //Required

lrv2.phoneAuthenticationApi.checkPhoneNumberAvailability(phone).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Remove Phone ID by Access Token (DELETE)

This API is used to delete the Phone ID on a user's account via the access token

 
 

 ```js

var accessToken = "<accessToken>"; //Required

lrv2.phoneAuthenticationApi.removePhoneIDByAccessToken(accessToken).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
 

### MultiFactorAuthentication API


List of APIs in this Section:
 
* PUT : [Update MFA by Access Token](#update-mfa-by-access-token-put) 
* PUT : [MFA Update Phone Number by Token](#mfa-update-phone-number-by-token-put) 
* PUT : [MFA Validate Google Auth Code](#mfa-validate-google-auth-code-put) 
* PUT : [MFA Update Phone Number](#mfa-update-phone-number-put) 
* POST : [MFA Email Login](#mfa-email-login-post) 
* POST : [MFA Phone Login](#mfa-phone-login-post) 
* GET : [MFA Validate Access Token](#mfa-validate-access-token-get) 
* GET : [MFA Resend Otp](#mfa-resend-otp-get) 
* DELETE : [MFA Reset Google Authenticator by Token](#mfa-reset-google-authenticator-by-token-delete) 
* DELETE : [MFA Reset SMS Authenticator by Token](#mfa-reset-sms-authenticator-by-token-delete) 
* DELETE : [MFA Reset SMS Authenticator By UID](#mfa-reset-sms-authenticator-by-uid-delete) 
* DELETE : [MFA Reset Google Authenticator By UID](#mfa-reset-google-authenticator-by-uid-delete)



#### Update MFA by Access Token (PUT)

This API is used to Enable Multi-factor authentication by access token on user login

 
 

 ```js

var accessToken = "<accessToken>"; //Required

var multiFactorAuthModelByGoogleAuthenticatorCode ={ 
"googleAuthenticatorCode" : "<googleAuthenticatorCode>"
};  //Required
var fields = null; //Optional
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.multiFactorAuthenticationApi.mfaUpdateByAccessToken(accessToken, multiFactorAuthModelByGoogleAuthenticatorCode, fields, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Update Phone Number by Token (PUT)

This API is used to update the Multi-factor authentication phone number by sending the verification OTP to the provided phone number

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var phoneNo2FA = "<phoneNo2FA>"; //Required
var smsTemplate2FA = "<smsTemplate2FA>"; //Optional

lrv2.multiFactorAuthenticationApi.mfaUpdatePhoneNumberByToken(accessToken, phoneNo2FA, smsTemplate2FA).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Validate Google Auth Code (PUT)

This API is used to login via Multi-factor-authentication by passing the google authenticator code.

 
 

 ```js

var googleAuthenticatorCode = "<googleAuthenticatorCode>"; //Required
var secondFactorAuthenticationToken = "<secondFactorAuthenticationToken>"; //Required
var fields = null; //Optional
var smsTemplate2FA = "<smsTemplate2FA>"; //Optional

lrv2.multiFactorAuthenticationApi.mfaValidateGoogleAuthCode(googleAuthenticatorCode, secondFactorAuthenticationToken, fields, smsTemplate2FA).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Update Phone Number (PUT)

This API is used to update (if configured) the phone number used for Multi-factor authentication by sending the verification OTP to the provided phone number

 
 

 ```js

var phoneNo2FA = "<phoneNo2FA>"; //Required
var secondFactorAuthenticationToken = "<secondFactorAuthenticationToken>"; //Required
var smsTemplate2FA = "<smsTemplate2FA>"; //Optional

lrv2.multiFactorAuthenticationApi.mfaUpdatePhoneNumber(phoneNo2FA, secondFactorAuthenticationToken, smsTemplate2FA).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Email Login (POST)

This API can be used to login by emailid on a Multi-factor authentication enabled LoginRadius site.

 
 

 ```js

var email = "<email>"; //Required
var password = "<password>"; //Required
var emailTemplate = "<emailTemplate>"; //Optional
var fields = null; //Optional
var loginUrl = "<loginUrl>"; //Optional
var smsTemplate = "<smsTemplate>"; //Optional
var smsTemplate2FA = "<smsTemplate2FA>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional

lrv2.multiFactorAuthenticationApi.mfaLoginByEmail(email, password, emailTemplate, fields, loginUrl, smsTemplate, smsTemplate2FA, verificationUrl).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Phone Login (POST)

This API can be used to login by Phone on a Multi-factor authentication enabled LoginRadius site.

 
 

 ```js

var password = "<password>"; //Required
var phone = "<phone>"; //Required
var emailTemplate = "<emailTemplate>"; //Optional
var fields = null; //Optional
var loginUrl = "<loginUrl>"; //Optional
var smsTemplate = "<smsTemplate>"; //Optional
var smsTemplate2FA = "<smsTemplate2FA>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional

lrv2.multiFactorAuthenticationApi.mfaLoginByPhone(password, phone, emailTemplate, fields, loginUrl, smsTemplate, smsTemplate2FA, verificationUrl).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Validate Access Token (GET)

This API is used to configure the Multi-factor authentication after login by using the access token when MFA is set as optional on the LoginRadius site.

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var smsTemplate2FA = "<smsTemplate2FA>"; //Optional

lrv2.multiFactorAuthenticationApi.mfaConfigureByAccessToken(accessToken, smsTemplate2FA).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Resend Otp (GET)

This API is used to resending the verification OTP to the provided phone number

 
 

 ```js

var secondFactorAuthenticationToken = "<secondFactorAuthenticationToken>"; //Required
var smsTemplate2FA = "<smsTemplate2FA>"; //Optional

lrv2.multiFactorAuthenticationApi.mfaResendOTP(secondFactorAuthenticationToken, smsTemplate2FA).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Reset Google Authenticator by Token (DELETE)

This API Resets the Google Authenticator configurations on a given account via the access token

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var googleauthenticator = true; //Required

lrv2.multiFactorAuthenticationApi.mfaResetGoogleAuthByToken(accessToken, googleauthenticator).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Reset SMS Authenticator by Token (DELETE)

This API resets the SMS Authenticator configurations on a given account via the access token.

 
 

 ```js

var accessToken = "<accessToken>"; //Required
var otpauthenticator = true; //Required

lrv2.multiFactorAuthenticationApi.mfaResetSMSAuthByToken(accessToken, otpauthenticator).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Reset SMS Authenticator By UID (DELETE)

This API resets the SMS Authenticator configurations on a given account via the UID.

 
 

 ```js

var otpauthenticator = true; //Required
var uid = "<uid>"; //Required

lrv2.multiFactorAuthenticationApi.mfaResetSMSAuthenticatorByUid(otpauthenticator, uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### MFA Reset Google Authenticator By UID (DELETE)

This API resets the Google Authenticator configurations on a given account via the UID.

 
 

 ```js

var googleauthenticator = true; //Required
var uid = "<uid>"; //Required

lrv2.multiFactorAuthenticationApi.mfaResetGoogleAuthenticatorByUid(googleauthenticator, uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
 

### PasswordLessLogin API


List of APIs in this Section:
 
* PUT : [Passwordless Login Phone Verification](#passwordless-login-phone-verification-put) 
* GET : [Passwordless Login by Phone](#passwordless-login-by-phone-get) 
* GET : [Passwordless Login By Email](#passwordless-login-by-email-get) 
* GET : [Passwordless Login Verification](#passwordless-login-verification-get)



#### Passwordless Login Phone Verification (PUT)

This API verifies an account by OTP and allows the customer to login.

 
 

 ```js


var passwordLessLoginOtpModel ={ 
"otp" : "<otp>",
"phone" : "<phone>"
};  //Required
var fields = null; //Optional
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.passwordLessLoginApi.passwordlessLoginPhoneVerification(passwordLessLoginOtpModel, fields, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Passwordless Login by Phone (GET)

API can be used to send a One-time Passcode (OTP) provided that the account has a verified PhoneID

 
 

 ```js

var phone = "<phone>"; //Required
var smsTemplate = "<smsTemplate>"; //Optional

lrv2.passwordLessLoginApi.passwordlessLoginByPhone(phone, smsTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Passwordless Login By Email (GET)

This API is used to send a Passwordless Login verification link to the provided Email ID

 
 

 ```js

var email = "<email>"; //Required
var passwordLessLoginTemplate = "<passwordLessLoginTemplate>"; //Optional
var verificationUrl = "<verificationUrl>"; //Optional

lrv2.passwordLessLoginApi.passwordlessLoginByEmail(email, passwordLessLoginTemplate, verificationUrl).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Passwordless Login Verification (GET)

This API is used to verify the Passwordless Login verification link. Note: If you are using Passwordless Login by Phone you will need to use the Passwordless Login Phone Verification API

 
 

 ```js

var verificationToken = "<verificationToken>"; //Required
var fields = null; //Optional
var welcomeEmailTemplate = "<welcomeEmailTemplate>"; //Optional

lrv2.passwordLessLoginApi.passwordlessLoginVerification(verificationToken, fields, welcomeEmailTemplate).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
 

### Role API


List of APIs in this Section:
 
* PUT : [Assign Roles by UID](#assign-roles-by-uid-put) 
* PUT : [Upsert Context](#upsert-context-put) 
* PUT : [Add Permissions to Role](#add-permissions-to-role-put) 
* POST : [Roles Create](#roles-create-post) 
* GET : [Roles by UID](#roles-by-uid-get) 
* GET : [Get Context with Roles and Permissions](#get-context-with-roles-and-permissions-get) 
* GET : [Role Context profile](#role-context-profile-get) 
* GET : [Roles List](#roles-list-get) 
* DELETE : [Unassign Roles by UID](#unassign-roles-by-uid-delete) 
* DELETE : [Delete Role Context](#delete-role-context-delete) 
* DELETE : [Delete Role from Context](#delete-role-from-context-delete) 
* DELETE : [Delete Additional Permission from Context](#delete-additional-permission-from-context-delete) 
* DELETE : [Account Delete Role](#account-delete-role-delete) 
* DELETE : [Remove Permissions](#remove-permissions-delete)



#### Assign Roles by UID (PUT)

This API is used to assign your desired roles to a given user.

 
 

 ```js


var accountRolesModel ={ 
"roles" : [  "roles" ] 
};  //Required
var uid = "<uid>"; //Required

lrv2.roleApi.assignRolesByUid(accountRolesModel, uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Upsert Context (PUT)

This API creates a Context with a set of Roles

 
 

 ```js


var accountRoleContextModel ={ 
"roleContext" : [   { 
  "additionalPermissions" : ["<additionalPermissions>" ] ,
 "context" : "<context>"  ,
 "expiration" : "<expiration>"  ,
  "roles" : ["<roles>" ]  
}  ] 
};  //Required
var uid = "<uid>"; //Required

lrv2.roleApi.updateRoleContextByUid(accountRoleContextModel, uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Add Permissions to Role (PUT)

This API is used to add permissions to a given role.

 
 

 ```js


var permissionsModel ={ 
"permissions" : [  "permissions" ] 
};  //Required
var role = "<role>"; //Required

lrv2.roleApi.addRolePermissions(permissionsModel, role).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Roles Create (POST)

This API creates a role with permissions.

 
 

 ```js


var rolesModel ={ 
"roles" : [   { 
 "name" : "<name>"  ,
"permissions" : {"Permission_name":true}  
}  ] 
};  //Required

lrv2.roleApi.createRoles(rolesModel).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Roles by UID (GET)

API is used to retrieve all the assigned roles of a particular User.

 
 

 ```js

var uid = "<uid>"; //Required

lrv2.roleApi.getRolesByUid(uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Get Context with Roles and Permissions (GET)

This API Gets the contexts that have been configured and the associated roles and permissions.

 
 

 ```js

var uid = "<uid>"; //Required

lrv2.roleApi.getRoleContextByUid(uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Role Context profile (GET)

The API is used to retrieve role context by the context name.

 
 

 ```js

var contextName = "<contextName>"; //Required

lrv2.roleApi.getRoleContextByContextName(contextName).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Roles List (GET)

This API retrieves the complete list of created roles with permissions of your app.

 
 

 ```js


lrv2.roleApi.getRolesList().then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Unassign Roles by UID (DELETE)

This API is used to unassign roles from a user.

 
 

 ```js


var accountRolesModel ={ 
"roles" : [  "roles" ] 
};  //Required
var uid = "<uid>"; //Required

lrv2.roleApi.unassignRolesByUid(accountRolesModel, uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Delete Role Context (DELETE)

This API Deletes the specified Role Context

 
 

 ```js

var contextName = "<contextName>"; //Required
var uid = "<uid>"; //Required

lrv2.roleApi.deleteRoleContextByUid(contextName, uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Delete Role from Context (DELETE)

This API Deletes the specified Role from a Context.

 
 

 ```js

var contextName = "<contextName>"; //Required

var roleContextRemoveRoleModel ={ 
"roles" : [  "roles" ] 
};  //Required
var uid = "<uid>"; //Required

lrv2.roleApi.deleteRolesFromRoleContextByUid(contextName, roleContextRemoveRoleModel, uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Delete Additional Permission from Context (DELETE)

This API Deletes Additional Permissions from Context.

 
 

 ```js

var contextName = "<contextName>"; //Required

var roleContextAdditionalPermissionRemoveRoleModel ={ 
"additionalPermissions" : [  "additionalPermissions" ] 
};  //Required
var uid = "<uid>"; //Required

lrv2.roleApi.deleteAdditionalPermissionFromRoleContextByUid(contextName, roleContextAdditionalPermissionRemoveRoleModel, uid).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Account Delete Role (DELETE)

This API is used to delete the role.

 
 

 ```js

var role = "<role>"; //Required

lrv2.roleApi.deleteRole(role).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Remove Permissions (DELETE)

API is used to remove permissions from a role.

 
 

 ```js


var permissionsModel ={ 
"permissions" : [  "permissions" ] 
};  //Required
var role = "<role>"; //Required

lrv2.roleApi.removeRolePermissions(permissionsModel, role).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
 

### Sott API


List of APIs in this Section:
 
* GET : [Generate SOTT](#generate-sott-get)



#### Generate SOTT (GET)

This API allows you to generate SOTT with a given expiration time.

 
 

 ```js

var timeDifference = 0; //Optional

lrv2.sottApi.generateSott(timeDifference).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
 

### NativeSocial API


List of APIs in this Section:
 
* GET : [Access Token via Facebook Token](#access-token-via-facebook-token-get) 
* GET : [Access Token via Twitter Token](#access-token-via-twitter-token-get) 
* GET : [Access Token via Google Token](#access-token-via-google-token-get) 
* GET : [Access Token using google JWT token for Native Mobile Login](#access-token-using-google-jwt-token-for-native-mobile-login-get) 
* GET : [Access Token via Linkedin Token](#access-token-via-linkedin-token-get) 
* GET : [Access Token via Google AuthCode](#access-token-via-google-authcode-get)



#### Access Token via Facebook Token (GET)

The API is used to get LoginRadius access token by sending Facebook's access token. It will be valid for the specific duration of time specified in the response.

 
 

 ```js

var fbAccessToken = "<fbAccessToken>"; //Required

lrv2.nativeSocialApi.getAccessTokenByFacebookAccessToken(fbAccessToken).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Access Token via Twitter Token (GET)

The API is used to get LoginRadius access token by sending Twitter's access token. It will be valid for the specific duration of time specified in the response.

 
 

 ```js

var twAccessToken = "<twAccessToken>"; //Required
var twTokenSecret = "<twTokenSecret>"; //Required

lrv2.nativeSocialApi.getAccessTokenByTwitterAccessToken(twAccessToken, twTokenSecret).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Access Token via Google Token (GET)

The API is used to get LoginRadius access token by sending Google's access token. It will be valid for the specific duration of time specified in the response.

 
 

 ```js

var googleAccessToken = "<googleAccessToken>"; //Required
var clientId = "<clientId>"; //Optional
var refreshToken = "<refreshToken>"; //Optional

lrv2.nativeSocialApi.getAccessTokenByGoogleAccessToken(googleAccessToken, clientId, refreshToken).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Access Token using google JWT token for Native Mobile Login (GET)

This API is used to Get LoginRadius Access Token using google jwt id token for google native mobile login/registration.

 
 

 ```js

var idToken = "<idToken>"; //Required

lrv2.nativeSocialApi.getAccessTokenByGoogleJWTAccessToken(idToken).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Access Token via Linkedin Token (GET)

The API is used to get LoginRadius access token by sending Linkedin's access token. It will be valid for the specific duration of time specified in the response.

 
 

 ```js

var lnAccessToken = "<lnAccessToken>"; //Required

lrv2.nativeSocialApi.getAccessTokenByLinkedinAccessToken(lnAccessToken).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Access Token via Google AuthCode (GET)

The API is used to get LoginRadius access token by sending Google's AuthCode. It will be valid for the specific duration of time specified in the response.

 
 

 ```js

var googleAuthcode = "<googleAuthcode>"; //Required

lrv2.nativeSocialApi.getAccessTokenByGoogleAuthCode(googleAuthcode).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
 

### WebHook API


List of APIs in this Section:
 
* POST : [Webhook Subscribe](#webhook-subscribe-post) 
* GET : [Webhook Subscribed URLs](#webhook-subscribed-urls-get) 
* GET : [Webhook Test](#webhook-test-get) 
* DELETE : [WebHook Unsubscribe](#webhook-unsubscribe-delete)



#### Webhook Subscribe (POST)

API can be used to configure a WebHook on your LoginRadius site. Webhooks also work on subscribe and notification model, subscribe your hook and get a notification. Equivalent to RESThook but these provide security on basis of signature and RESThook work on unique URL. Following are the events that are allowed by LoginRadius to trigger a WebHook service call.

 
 

 ```js


var webHookSubscribeModel ={ 
"event" : "<event>",
"targetUrl" : "<targetUrl>"
};  //Required

lrv2.webHookApi.webHookSubscribe(webHookSubscribeModel).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Webhook Subscribed URLs (GET)

This API is used to fatch all the subscribed URLs, for particular event

 
 

 ```js

var event = "<event>"; //Required

lrv2.webHookApi.getWebHookSubscribedURLs(event).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### Webhook Test (GET)

API can be used to test a subscribed WebHook.

 
 

 ```js


lrv2.webHookApi.webhookTest().then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```
 
  
  
 
#### WebHook Unsubscribe (DELETE)

API can be used to unsubscribe a WebHook configured on your LoginRadius site.

 
 

 ```js


var webHookSubscribeModel ={ 
"event" : "<event>",
"targetUrl" : "<targetUrl>"
};  //Required

lrv2.webHookApi.webHookUnsubscribe(webHookSubscribeModel).then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error);
});

 ```








[Go Back to Home Page](/)
