---
title: Ruby Library
description: "A guide to implement LoginRadius SDK in a Ruby on Rails application."
summary: "A guide to implement LoginRadius SDK in a Ruby on Rails application"
path: "/references/sdk/ruby-sdk"
---
# Ruby SDK

>**Disclaimer:** This library is meant to help you with a quick implementation of the LoginRadius platform and also to serve as a reference point for the LoginRadius API. Keep in mind that it is an open source library, which means you are free to download and customize the library functions based on your specific application needs.

This document contains information and examples regarding the LoginRadius ruby-on-rails SDK. It provides guidance for working with social authentication, user profile data, and auth implementation with a variety of social networks such as Facebook, Google, Twitter.You can get the SDK from [here](https://github.com/LoginRadius/ruby-on-rails-sdk).

For a more hands-on tutorial on setting up the SDK and using it in a Ruby application to make LoginRadius API calls, check out our Ruby [tutorial](/tutorial/ror).

## SDK Installation and Configuration

Add this line to your application's Gemfile:

```
gem 'login_radius', '~> 11.0'

```

And then execute:

```
bundle install

```

import {
  ExpansionPanel,
  ExpansionPanelList,
  ExpansionPanelListItem
} from 'gatsby-theme-apollo-docs';

import Setup from "../howto/dashboard-setup"

Before using any of the functions available in the library, its corresponding module must first define the global constant in `config/application.yml`:

```yml
  SITE_NAME: "<site name>"
  API_KEY: "<api-key>"
  API_SECRET: "<api-secret>"
  CUSTOM_API_DOMAIN: "<custom-domain-url-if-any>"
  API_REQUEST_SIGNING: "false"
```

> **Note:** You can get the API Key, API Secret and App/Site Name from the [Configuration > API Credentials](https://dashboard.loginradius.com/configuration) section of your LoginRadius Dashboard.

After define the configuration key details, its corresponding module must first be instantiated:

Create login_radius.rb in /config/initializers:

```ruby
require 'login_radius'

::AccountApi = LoginRadius::AccountApi.new
::RoleApi = LoginRadius::RoleApi.new
::SottApi = LoginRadius::SottApi.new

::ConfigurationApi = LoginRadius::ConfigurationApi.new
::MultiFactorAuthenticationApi = LoginRadius::MultiFactorAuthenticationApi.new
::WebHookApi = LoginRadius::WebHookApi.new

::AuthenticationApi = LoginRadius::AuthenticationApi.new
::PasswordLessLoginApi = LoginRadius::PasswordLessLoginApi.new
::PhoneAuthenticationApi = LoginRadius::PhoneAuthenticationApi.new

::NativeSocialApi = LoginRadius::NativeSocialApi.new
::SocialApi = LoginRadius::SocialApi.new

```

For more details check API Reference [here](/#api)

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

 
 

 ```

 access_token = "<access_token>" #Required

 user_profile_update_model ={ 
|FirstName|user's first name
|LastName|user's last name  }  #Required
 email_template = "<email_template>" #Optional
 fields = nil #Optional
 null_support = true #Optional
 sms_template = "<sms_template>" #Optional
 verification_url = "<verification_url>" #Optional

response = AuthenticationApi.update_profile_by_access_token(access_token, user_profile_update_model, email_template, fields, null_support, sms_template, verification_url)

 ```
 
  
  
 
#### Auth Verify Email By OTP (PUT)

This API is used to verify the email of user when the OTP Email verification flow is enabled, please note that you must contact LoginRadius to have this feature enabled.   

 
 

 ```


 email_verification_by_otp_model ={ 
|Email|user's email
|Otp|The Verification Code  }  #Required
 fields = nil #Optional
 url = "<url>" #Optional
 welcome_email_template = "<welcome_email_template>" #Optional

response = AuthenticationApi.verify_email_by_otp(email_verification_by_otp_model, fields, url, welcome_email_template)

 ```
 
  
  
 
#### Auth Reset Password by Reset Token (PUT)

This API is used to set a new password for the specified account.   

 
 

 ```


 reset_password_by_reset_token_model ={ 
|Password|Password for the email
|ResetToken|reset token received in the email  }  #Required

response = AuthenticationApi.reset_password_by_reset_token(reset_password_by_reset_token_model)

 ```
 
  
  
 
#### Auth Change Password (PUT)

This API is used to change the accounts password based on the previous password   

 
 

 ```

 access_token = "<access_token>" #Required
 new_password = "<new_password>" #Required
 old_password = "<old_password>" #Required

response = AuthenticationApi.change_password(access_token, new_password, old_password)

 ```
 
  
  
 
#### Auth Resend Email Verification (PUT)

This API resends the verification email to the user.   

 
 

 ```

 email = "<email>" #Required
 email_template = "<email_template>" #Optional
 verification_url = "<verification_url>" #Optional

response = AuthenticationApi.auth_resend_email_verification(email, email_template, verification_url)

 ```
 
  
  
 
#### Auth Add Email (POST)

This API is used to add additional emails to a user's account.   

 
 

 ```

 access_token = "<access_token>" #Required
 email = "<email>" #Required
 type = "<type>" #Required
 email_template = "<email_template>" #Optional
 verification_url = "<verification_url>" #Optional

response = AuthenticationApi.add_email(access_token, email, type, email_template, verification_url)

 ```
 
  
  
 
#### Auth Login by Email (POST)

This API retrieves a copy of the user data based on the Email   

 
 

 ```


 email_authentication_model ={ 
|email|user's email
|password|Password for the email  }  #Required
 email_template = "<email_template>" #Optional
 fields = nil #Optional
 login_url = "<login_url>" #Optional
 verification_url = "<verification_url>" #Optional

response = AuthenticationApi.login_by_email(email_authentication_model, email_template, fields, login_url, verification_url)

 ```
 
  
  
 
#### Auth Forgot Password (POST)

This API is used to send the reset password url to a specified account. Note: If you have the UserName workflow enabled, you may replace the 'email' parameter with 'username'   

 
 

 ```

 email = "<email>" #Required
 reset_password_url = "<reset_password_url>" #Required
 email_template = "<email_template>" #Optional

response = AuthenticationApi.forgot_password(email, reset_password_url, email_template)

 ```
 
  
  
 
#### Auth User Registration by Email (POST)

This API creates a user in the database as well as sends a verification email to the user.   

 
 

 ```


 auth_user_registration_model ={ 
|Email|boolean type value, default is true
|FirstName|user's first name
|LastName|user's last name
|Password|Password for the email  }  #Required
 sott = "<sott>" #Required
 email_template = "<email_template>" #Optional
 fields = nil #Optional
 options = "<options>" #Optional
 verification_url = "<verification_url>" #Optional
 welcome_email_template = "<welcome_email_template>" #Optional

response = AuthenticationApi.user_registration_by_email(auth_user_registration_model, sott, email_template, fields, options, verification_url, welcome_email_template)

 ```
 
  
  
 
#### Auth User Registration By Captcha (POST)

This API creates a user in the database as well as sends a verification email to the user.   

 
 

 ```


 auth_user_registration_model_with_captcha ={ 
|Email|boolean type value, default is true
|FirstName|user's first name
|g-recaptcha-response|The acknowledgement received by Google in Google recaptcha authorisation process.
|LastName|user's last name
|Password|Password for the email  }  #Required
 email_template = "<email_template>" #Optional
 fields = nil #Optional
 options = "<options>" #Optional
 sms_template = "<sms_template>" #Optional
 verification_url = "<verification_url>" #Optional
 welcome_email_template = "<welcome_email_template>" #Optional

response = AuthenticationApi.user_registration_by_captcha(auth_user_registration_model_with_captcha, email_template, fields, options, sms_template, verification_url, welcome_email_template)

 ```
 
  
  
 
#### Auth Validate Access token (GET)

This api validates access token, if valid then returns a response with its expiry otherwise error.   

 
 

 ```

 access_token = "<access_token>" #Required

response = AuthenticationApi.auth_validate_access_token(access_token)

 ```
 
  
  
 
#### Access Token Invalidate (GET)

This api call invalidates the active access token or expires an access token's validity.   

 
 

 ```

 access_token = "<access_token>" #Required
 prevent_refresh = true #Optional

response = AuthenticationApi.auth_in_validate_access_token(access_token, prevent_refresh)

 ```
 
  
  
 
#### Access Token Info (GET)

This api call provides the active access token Information.  

 
 

 ```

 access_token = "<access_token>" #Required

response = AuthenticationApi.get_access_token_info(access_token)

 ```
 
  
  
 
#### Auth Read all Profiles by Token (GET)

This API retrieves a copy of the user data based on the access token.   

 
 

 ```

 access_token = "<access_token>" #Required
 fields = nil #Optional

response = AuthenticationApi.get_profile_by_access_token(access_token, fields)

 ```
 
  
  
 
#### Auth Send Welcome Email (GET)

This API sends a welcome email.

 
 

 ```

 access_token = "<access_token>" #Required
 welcome_email_template = "<welcome_email_template>" #Optional

response = AuthenticationApi.send_welcome_email(access_token, welcome_email_template)

 ```
 
  
  
 
#### Auth Delete Account (GET)

This API is used to delete an account by passing it a delete token.   

 
 

 ```

 deletetoken = "<deletetoken>" #Required

response = AuthenticationApi.delete_account_by_delete_token(deletetoken)

 ```
 
  
  
 
#### Auth Check Email Availability (GET)

This API is used to check if the email exists or not on your site.   

 
 

 ```

 email = "<email>" #Required

response = AuthenticationApi.check_email_availability(email)

 ```
 
  
  
 
#### Auth Verify Email (GET)

This API is used to verify the email of user. Note: This API will only return the full profile if you have 'Enable auto login after email verification' set in your LoginRadius Admin Console's Email Workflow settings under 'Verification Email'.   

 
 

 ```

 verification_token = "<verification_token>" #Required
 fields = nil #Optional
 url = "<url>" #Optional
 welcome_email_template = "<welcome_email_template>" #Optional

response = AuthenticationApi.verify_email(verification_token, fields, url, welcome_email_template)

 ```
 
  
  
 
#### Auth Social Identity (GET)

This API is called just after account linking API and it prevents the raas profile of the second account from getting created.   

 
 

 ```

 access_token = "<access_token>" #Required
 fields = nil #Optional

response = AuthenticationApi.get_social_identity(access_token, fields)

 ```
 
  
  
 
#### Auth Privacy Policy Accept (GET)

This API is used to update the privacy policy stored in the user's profile by providing the access token of the user accepting the privacy policy   

 
 

 ```

 access_token = "<access_token>" #Required
 fields = nil #Optional

response = AuthenticationApi.accept_privacy_policy(access_token, fields)

 ```
 
  
  
 
#### Auth Delete Account with Email Confirmation (DELETE)

This API will send a confirmation email for account deletion to the customer's email when passed the customer's access token   

 
 

 ```

 access_token = "<access_token>" #Required
 delete_url = "<delete_url>" #Optional
 email_template = "<email_template>" #Optional

response = AuthenticationApi.delete_account_with_email_confirmation(access_token, delete_url, email_template)

 ```
 
  
  
 
#### Auth Remove Email (DELETE)

This API is used to remove additional emails from a user's account.   

 
 

 ```

 access_token = "<access_token>" #Required
 email = "<email>" #Required

response = AuthenticationApi.remove_email(access_token, email)

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

 
 

 ```


 account_user_profile_update_model ={ 
|FirstName|user's first name
|LastName|user's last name  }  #Required
 uid = "<uid>" #Required
 fields = nil #Optional
 null_support = true #Optional

response = AccountApi.update_account_by_uid(account_user_profile_update_model, uid, fields, null_support)

 ```
 
  
  
 
#### Update Phone ID by UID (PUT)

This API is used to update the PhoneId by using the Uid's. Admin can update the PhoneId's for both the verified and unverified profiles. It will directly replace the PhoneId and bypass the OTP verification process.   

 
 

 ```

 phone = "<phone>" #Required
 uid = "<uid>" #Required
 fields = nil #Optional

response = AccountApi.update_phone_id_by_uid(phone, uid, fields)

 ```
 
  
  
 
#### Account Set Password (PUT)

This API is used to set the password of an account in Cloud Storage.   

 
 

 ```

 password = "<password>" #Required
 uid = "<uid>" #Required

response = AccountApi.set_account_password_by_uid(password, uid)

 ```
 
  
  
 
#### Account Invalidate Verification Email (PUT)

This API is used to invalidate the Email Verification status on an account.   

 
 

 ```

 uid = "<uid>" #Required
 email_template = "<email_template>" #Optional
 verification_url = "<verification_url>" #Optional

response = AccountApi.invalidate_account_email_verification(uid, email_template, verification_url)

 ```
 
  
  
 
#### Reset phone ID verification (PUT)

This API Allows you to reset the phone number verification of an end user’s account.   

 
 

 ```

 uid = "<uid>" #Required
 sms_template = "<sms_template>" #Optional

response = AccountApi.reset_phone_id_verification_by_uid(uid, sms_template)

 ```
 
  
  
 
#### Upsert Email (PUT)

This API is used to add/upsert another emails in account profile by different-different email types. If the email type is same then it will simply update the existing email, otherwise it will add a new email in Email array.   

 
 

 ```


 upsert_email_model ={ 
|Email|user's email  }  #Required
 uid = "<uid>" #Required
 fields = nil #Optional

response = AccountApi.upsert_email(upsert_email_model, uid, fields)

 ```
 
  
  
 
#### Update UID (PUT)

This API is used to update a user's Uid. It will update all profiles, custom objects and consent management logs associated with the Uid.   

 
 

 ```


 update_uid_model ={ 
|NewUid|New Uid  }  #Required
 uid = "<uid>" #Required

response = AccountApi.account_update_uid(update_uid_model, uid)

 ```
 
  
  
 
#### Account Create (POST)

This API is used to create an account in Cloud Storage. This API bypasses the normal email verification process and manually creates the user. 
 
In order to use this API, you need to format a JSON request body with all of the mandatory fields   

 
 

 ```


 account_create_model ={ 
|Email|boolean type value, default is true
|FirstName|user's first name
|LastName|user's last name
|Password|Password for the email  }  #Required
 fields = nil #Optional

response = AccountApi.create_account(account_create_model, fields)

 ```
 
  
  
 
#### Forgot Password token (POST)

This API Returns a Forgot Password Token it can also be used to send a Forgot Password email to the customer. Note: If you have the UserName workflow enabled, you may replace the 'email' parameter with 'username' in the body.   

 
 

 ```

 email = "<email>" #Required
 email_template = "<email_template>" #Optional
 reset_password_url = "<reset_password_url>" #Optional
 send_email = true #Optional

response = AccountApi.get_forgot_password_token(email, email_template, reset_password_url, send_email)

 ```
 
  
  
 
#### Email Verification token (POST)

This API Returns an Email Verification token.   

 
 

 ```

 email = "<email>" #Required

response = AccountApi.get_email_verification_token(email)

 ```
 
  
  
 
#### Account Profiles by Email (GET)

This API is used to retrieve all of the profile data, associated with the specified account by email in Cloud Storage.   

 
 

 ```

 email = "<email>" #Required
 fields = nil #Optional

response = AccountApi.get_account_profile_by_email(email, fields)

 ```
 
  
  
 
#### Account Profile by Phone ID (GET)

This API is used to retrieve all of the profile data, associated with the account by phone number in Cloud Storage.   

 
 

 ```

 phone = "<phone>" #Required
 fields = nil #Optional

response = AccountApi.get_account_profile_by_phone(phone, fields)

 ```
 
  
  
 
#### Account Profiles by UID (GET)

This API is used to retrieve all of the profile data, associated with the account by uid in Cloud Storage.   

 
 

 ```

 uid = "<uid>" #Required
 fields = nil #Optional

response = AccountApi.get_account_profile_by_uid(uid, fields)

 ```
 
  
  
 
#### Account Password (GET)

This API is used to retrieve the hashed password of a specified account in Cloud Storage.   

 
 

 ```

 uid = "<uid>" #Required

response = AccountApi.get_account_password_hash_by_uid(uid)

 ```
 
  
  
 
#### Access Token based on UID or User impersonation API (GET)

The API is used to get LoginRadius access token based on UID.   

 
 

 ```

 uid = "<uid>" #Required

response = AccountApi.get_access_token_by_uid(uid)

 ```
 
  
  
 
#### Account Identities by Email (GET)

Note: This is intended for specific workflows where an email may be associated to multiple UIDs. This API is used to retrieve all of the identities (UID and Profiles), associated with a specified email in Cloud Storage.   

 
 

 ```

 email = "<email>" #Required
 fields = nil #Optional

response = AccountApi.get_account_identities_by_email(email, fields)

 ```
 
  
  
 
#### Account Delete (DELETE)

This API deletes the Users account and allows them to re-register for a new account.   

 
 

 ```

 uid = "<uid>" #Required

response = AccountApi.delete_account_by_uid(uid)

 ```
 
  
  
 
#### Account Remove Email (DELETE)

Use this API to Remove emails from a user Account   

 
 

 ```

 email = "<email>" #Required
 uid = "<uid>" #Required
 fields = nil #Optional

response = AccountApi.remove_email(email, uid, fields)

 ```
 
  
  
 
#### Delete User Profiles By Email (DELETE)

This API is used to delete all user profiles associated with an Email.   

 
 

 ```

 email = "<email>" #Required

response = AccountApi.account_delete_by_email(email)

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

This API is used to reset the password   

 
 

 ```


 reset_password_by_otp_model ={ 
|otp|The Verification Code
|password|Password for the email
|Phone|New Phone Number  }  #Required

response = PhoneAuthenticationApi.reset_password_by_phone_otp(reset_password_by_otp_model)

 ```
 
  
  
 
#### Phone Verification OTP (PUT)

This API is used to validate the verification code sent to verify a user's phone number   

 
 

 ```

 otp = "<otp>" #Required
 phone = "<phone>" #Required
 fields = nil #Optional
 sms_template = "<sms_template>" #Optional

response = PhoneAuthenticationApi.phone_verification_by_otp(otp, phone, fields, sms_template)

 ```
 
  
  
 
#### Phone Verification OTP by Token (PUT)

This API is used to consume the verification code sent to verify a user's phone number. Use this call for front-end purposes in cases where the user is already logged in by passing the user's access token.   

 
 

 ```

 access_token = "<access_token>" #Required
 otp = "<otp>" #Required
 sms_template = "<sms_template>" #Optional

response = PhoneAuthenticationApi.phone_verification_otp_by_access_token(access_token, otp, sms_template)

 ```
 
  
  
 
#### Phone Number Update (PUT)

This API is used to update the login Phone Number of users   

 
 

 ```

 access_token = "<access_token>" #Required
 phone = "<phone>" #Required
 sms_template = "<sms_template>" #Optional

response = PhoneAuthenticationApi.update_phone_number(access_token, phone, sms_template)

 ```
 
  
  
 
#### Phone Login (POST)

This API retrieves a copy of the user data based on the Phone   

 
 

 ```


 phone_authentication_model ={ 
|password|Password for the email
|phone|New Phone Number  }  #Required
 fields = nil #Optional
 login_url = "<login_url>" #Optional
 sms_template = "<sms_template>" #Optional

response = PhoneAuthenticationApi.login_by_phone(phone_authentication_model, fields, login_url, sms_template)

 ```
 
  
  
 
#### Phone Forgot Password by OTP (POST)

This API is used to send the OTP to reset the account password.   

 
 

 ```

 phone = "<phone>" #Required
 sms_template = "<sms_template>" #Optional

response = PhoneAuthenticationApi.forgot_password_by_phone_otp(phone, sms_template)

 ```
 
  
  
 
#### Phone Resend Verification OTP (POST)

This API is used to resend a verification OTP to verify a user's Phone Number. The user will receive a verification code that they will need to input   

 
 

 ```

 phone = "<phone>" #Required
 sms_template = "<sms_template>" #Optional

response = PhoneAuthenticationApi.phone_resend_verification_otp(phone, sms_template)

 ```
 
  
  
 
#### Phone Resend Verification OTP By Token (POST)

This API is used to resend a verification OTP to verify a user's Phone Number in cases in which an active token already exists   

 
 

 ```

 access_token = "<access_token>" #Required
 phone = "<phone>" #Required
 sms_template = "<sms_template>" #Optional

response = PhoneAuthenticationApi.phone_resend_verification_otp_by_token(access_token, phone, sms_template)

 ```
 
  
  
 
#### Phone User Registration by SMS (POST)

This API registers the new users into your Cloud Storage and triggers the phone verification process.   

 
 

 ```


 auth_user_registration_model ={ 
|FirstName|user's first name
|LastName|user's last name
|Password|Password for the email
|PhoneId|Phone ID (Unique Phone Number Identifier of the user)  }  #Required
 sott = "<sott>" #Required
 fields = nil #Optional
 options = "<options>" #Optional
 sms_template = "<sms_template>" #Optional
 verification_url = "<verification_url>" #Optional
 welcome_email_template = "<welcome_email_template>" #Optional

response = PhoneAuthenticationApi.user_registration_by_phone(auth_user_registration_model, sott, fields, options, sms_template, verification_url, welcome_email_template)

 ```
 
  
  
 
#### Phone Number Availability (GET)

This API is used to check if the Phone Number exists or not on your site.   

 
 

 ```

 phone = "<phone>" #Required

response = PhoneAuthenticationApi.check_phone_number_availability(phone)

 ```
 
  
  
 
#### Remove Phone ID by Access Token (DELETE)

This API is used to delete the Phone ID on a user's account via the access token   

 
 

 ```

 access_token = "<access_token>" #Required

response = PhoneAuthenticationApi.remove_phone_id_by_access_token(access_token)

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

 
 

 ```

 access_token = "<access_token>" #Required

 multi_factor_auth_model_by_google_authenticator_code ={ 
|GoogleAuthenticatorCode|The code generated by google authenticator app after scanning QR code  }  #Required
 fields = nil #Optional
 sms_template = "<sms_template>" #Optional

response = MultiFactorAuthenticationApi.mfa_update_by_access_token(access_token, multi_factor_auth_model_by_google_authenticator_code, fields, sms_template)

 ```
 
  
  
 
#### MFA Update Phone Number by Token (PUT)

This API is used to update the Multi-factor authentication phone number by sending the verification OTP to the provided phone number   

 
 

 ```

 access_token = "<access_token>" #Required
 phone_no_2fa = "<phone_no_2fa>" #Required
 sms_template_2fa = "<sms_template_2fa>" #Optional

response = MultiFactorAuthenticationApi.mfa_update_phone_number_by_token(access_token, phone_no_2fa, sms_template_2fa)

 ```
 
  
  
 
#### MFA Validate Google Auth Code (PUT)

This API is used to login via Multi-factor-authentication by passing the google authenticator code.   

 
 

 ```

 google_authenticator_code = "<google_authenticator_code>" #Required
 second_factor_authentication_token = "<second_factor_authentication_token>" #Required
 fields = nil #Optional
 sms_template_2fa = "<sms_template_2fa>" #Optional

response = MultiFactorAuthenticationApi.mfa_validate_google_auth_code(google_authenticator_code, second_factor_authentication_token, fields, sms_template_2fa)

 ```
 
  
  
 
#### MFA Update Phone Number (PUT)

This API is used to update (if configured) the phone number used for Multi-factor authentication by sending the verification OTP to the provided phone number   

 
 

 ```

 phone_no_2fa = "<phone_no_2fa>" #Required
 second_factor_authentication_token = "<second_factor_authentication_token>" #Required
 sms_template_2fa = "<sms_template_2fa>" #Optional

response = MultiFactorAuthenticationApi.mfa_update_phone_number(phone_no_2fa, second_factor_authentication_token, sms_template_2fa)

 ```
 
  
  
 
#### MFA Email Login (POST)

This API can be used to login by emailid on a Multi-factor authentication enabled LoginRadius site.   

 
 

 ```

 email = "<email>" #Required
 password = "<password>" #Required
 email_template = "<email_template>" #Optional
 fields = nil #Optional
 login_url = "<login_url>" #Optional
 sms_template = "<sms_template>" #Optional
 sms_template_2fa = "<sms_template_2fa>" #Optional
 verification_url = "<verification_url>" #Optional

response = MultiFactorAuthenticationApi.mfa_login_by_email(email, password, email_template, fields, login_url, sms_template, sms_template_2fa, verification_url)

 ```
 
  
  
 
#### MFA Phone Login (POST)

This API can be used to login by Phone on a Multi-factor authentication enabled LoginRadius site.   

 
 

 ```

 password = "<password>" #Required
 phone = "<phone>" #Required
 email_template = "<email_template>" #Optional
 fields = nil #Optional
 login_url = "<login_url>" #Optional
 sms_template = "<sms_template>" #Optional
 sms_template_2fa = "<sms_template_2fa>" #Optional
 verification_url = "<verification_url>" #Optional

response = MultiFactorAuthenticationApi.mfa_login_by_phone(password, phone, email_template, fields, login_url, sms_template, sms_template_2fa, verification_url)

 ```
 
  
  
 
#### MFA Validate Access Token (GET)

This API is used to configure the Multi-factor authentication after login by using the access token when MFA is set as optional on the LoginRadius site.   

 
 

 ```

 access_token = "<access_token>" #Required
 sms_template_2fa = "<sms_template_2fa>" #Optional

response = MultiFactorAuthenticationApi.mfa_configure_by_access_token(access_token, sms_template_2fa)

 ```
 
  
  
 
#### MFA Resend Otp (GET)

This API is used to resending the verification OTP to the provided phone number   

 
 

 ```

 second_factor_authentication_token = "<second_factor_authentication_token>" #Required
 sms_template_2fa = "<sms_template_2fa>" #Optional

response = MultiFactorAuthenticationApi.mfa_resend_otp(second_factor_authentication_token, sms_template_2fa)

 ```
 
  
  
 
#### MFA Reset Google Authenticator by Token (DELETE)

This API Resets the Google Authenticator configurations on a given account via the access token   

 
 

 ```

 access_token = "<access_token>" #Required
 googleauthenticator = true #Required

response = MultiFactorAuthenticationApi.mfa_reset_google_auth_by_token(access_token, googleauthenticator)

 ```
 
  
  
 
#### MFA Reset SMS Authenticator by Token (DELETE)

This API resets the SMS Authenticator configurations on a given account via the access token.   

 
 

 ```

 access_token = "<access_token>" #Required
 otpauthenticator = true #Required

response = MultiFactorAuthenticationApi.mfa_reset_sms_auth_by_token(access_token, otpauthenticator)

 ```
 
  
  
 
#### MFA Reset SMS Authenticator By UID (DELETE)

This API resets the SMS Authenticator configurations on a given account via the UID.   

 
 

 ```

 otpauthenticator = true #Required
 uid = "<uid>" #Required

response = MultiFactorAuthenticationApi.mfa_reset_sms_authenticator_by_uid(otpauthenticator, uid)

 ```
 
  
  
 
#### MFA Reset Google Authenticator By UID (DELETE)

This API resets the Google Authenticator configurations on a given account via the UID.   

 
 

 ```

 googleauthenticator = true #Required
 uid = "<uid>" #Required

response = MultiFactorAuthenticationApi.mfa_reset_google_authenticator_by_uid(googleauthenticator, uid)

 ```
 
  
  
 
 

### PasswordLessLogin API


List of APIs in this Section:

* PUT : [Passwordless Login Phone Verification](#passwordless-login-phone-verification-put)
* GET : [Passwordless Login by Phone](#passwordless-login-by-phone-get)
* GET : [Passwordless Login By Email](#passwordless-login-by-email-get)
* GET : [Passwordless Login Verification](#passwordless-login-verification-get)



#### Passwordless Login Phone Verification (PUT)

This API verifies an account by OTP and allows the customer to login.   

 
 

 ```


 password_less_login_otp_model ={ 
|Otp|The Verification Code
|Phone|New Phone Number  }  #Required
 fields = nil #Optional
 sms_template = "<sms_template>" #Optional

response = PasswordLessLoginApi.passwordless_login_phone_verification(password_less_login_otp_model, fields, sms_template)

 ```
 
  
  
 
#### Passwordless Login by Phone (GET)

This API can be used to send a One-time Passcode (OTP) provided that the account has a verified PhoneID.   

 
 

 ```

 phone = "<phone>" #Required
 sms_template = "<sms_template>" #Optional

response = PasswordLessLoginApi.passwordless_login_by_phone(phone, sms_template)

 ```
 
  
  
 
#### Passwordless Login By Email (GET)

This API is used to send a Passwordless Login verification link to the provided Email ID.   

 
 

 ```

 email = "<email>" #Required
 password_less_login_template = "<password_less_login_template>" #Optional
 verification_url = "<verification_url>" #Optional

response = PasswordLessLoginApi.passwordless_login_by_email(email, password_less_login_template, verification_url)

 ```
 
  
  
 
#### Passwordless Login Verification (GET)

This API is used to verify the Passwordless Login verification link. 

> **Note:** If you are using Passwordless Login by Phone you will need to use the Passwordless Login Phone Verification API.

 
 

 ```

 verification_token = "<verification_token>" #Required
 fields = nil #Optional
 welcome_email_template = "<welcome_email_template>" #Optional

response = PasswordLessLoginApi.passwordless_login_verification(verification_token, fields, welcome_email_template)

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

 
 

 ```


 account_roles_model ={ 
|Roles|Array of String, which represents the role name  }  #Required
 uid = "<uid>" #Required

response = RoleApi.assign_roles_by_uid(account_roles_model, uid)

 ```
 
  
  
 
#### Upsert Context (PUT)

This API creates a Context with a set of Roles   

 
 

 ```


 account_role_context_model ={ 
|RoleContext|Array of RoleContext object, see body tab for structure  }  #Required
 uid = "<uid>" #Required

response = RoleApi.update_role_context_by_uid(account_role_context_model, uid)

 ```
 
  
  
 
#### Add Permissions to Role (PUT)

This API is used to add permissions to a given role.   

 
 

 ```


 permissions_model ={ 
|Permissions|Any Permission name for the role  }  #Required
 role = "<role>" #Required

response = RoleApi.add_role_permissions(permissions_model, role)

 ```
 
  
  
 
#### Roles Create (POST)

This API creates a role with permissions.   

 
 

 ```


 roles_model ={ 
|Roles|Array of String, which represents the role name  }  #Required

response = RoleApi.create_roles(roles_model)

 ```
 
  
  
 
#### Roles by UID (GET)

This API is used to retrieve all the assigned roles of a particular User.   

 
 

 ```

 uid = "<uid>" #Required

response = RoleApi.get_roles_by_uid(uid)

 ```
 
  
  
 
#### Get Context with Roles and Permissions (GET)

This API gets the contexts that have been configured and the associated roles and permissions.   

 
 

 ```

 uid = "<uid>" #Required

response = RoleApi.get_role_context_by_uid(uid)

 ```
 
  
  
 
#### Role Context profile (GET)

This API is used to retrieve role context by the context name.   

 
 

 ```

 context_name = "<context_name>" #Required

response = RoleApi.get_role_context_by_context_name(context_name)

 ```
 
  
  
 
#### Roles List (GET)

This API retrieves the complete list of created roles with permissions of your app.   

 
 

 ```


response = RoleApi.get_roles_list()

 ```
 
  
  
 
#### Unassign Roles by UID (DELETE)

This API is used to unassign roles from a user.   

 
 

 ```


 account_roles_model ={ 
|Roles|Array of String, which represents the role name  }  #Required
 uid = "<uid>" #Required

response = RoleApi.unassign_roles_by_uid(account_roles_model, uid)

 ```
 
  
  
 
#### Delete Role Context (DELETE)

This API Deletes the specified Role Context.  

 
 

 ```

 context_name = "<context_name>" #Required
 uid = "<uid>" #Required

response = RoleApi.delete_role_context_by_uid(context_name, uid)

 ```
 
  
  
 
#### Delete Role from Context (DELETE)

This API Deletes the specified Role from a Context.   

 
 

 ```

 context_name = "<context_name>" #Required

 role_context_remove_role_model ={ 
|Roles|Array of String, which represents the role name  }  #Required
 uid = "<uid>" #Required

response = RoleApi.delete_roles_from_role_context_by_uid(context_name, role_context_remove_role_model, uid)

 ```
 
  
  
 
#### Delete Additional Permission from Context (DELETE)

This API deletes additional permissions from Context.   

 
 

 ```

 context_name = "<context_name>" #Required

 role_context_additional_permission_remove_role_model ={ 
|AdditionalPermissions|Array of String, which represents the additional permissions  }  #Required
 uid = "<uid>" #Required

response = RoleApi.delete_additional_permission_from_role_context_by_uid(context_name, role_context_additional_permission_remove_role_model, uid)

 ```
 
  
  
 
#### Account Delete Role (DELETE)

This API is used to delete the role.   

 
 

 ```

 role = "<role>" #Required

response = RoleApi.delete_role(role)

 ```
 
  
  
 
#### Remove Permissions (DELETE)

This API is used to remove permissions from a role.   

 
 

 ```


 permissions_model ={ 
|Permissions|Any Permission name for the role  }  #Required
 role = "<role>" #Required

response = RoleApi.remove_role_permissions(permissions_model, role)

 ```
 
  
  
 
 

### Sott API


List of APIs in this Section:

* GET : [Generate SOTT](#generate-sott-get)



#### Generate SOTT (GET)

This API allows you to generate SOTT with a given expiration time.   

 
 

 ```

 time_difference = 0 #Optional

response = SottApi.generate_sott(time_difference)

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

 
 

 ```

 fb_access_token = "<fb_access_token>" #Required

response = NativeSocialApi.get_access_token_by_facebook_access_token(fb_access_token)

 ```
 
  
  
 
#### Access Token via Twitter Token (GET)

This API is used to get LoginRadius access token by sending Twitter's access token. It will be valid for the specific duration of time specified in the response.   

 
 

 ```

 tw_access_token = "<tw_access_token>" #Required
 tw_token_secret = "<tw_token_secret>" #Required

response = NativeSocialApi.get_access_token_by_twitter_access_token(tw_access_token, tw_token_secret)

 ```
 
  
  
 
#### Access Token via Google Token (GET)

This API is used to get LoginRadius access token by sending Google's access token. It will be valid for the specific duration of time specified in the response.   

 
 

 ```

 google_access_token = "<google_access_token>" #Required
 client_id = "<client_id>" #Optional
 refresh_token = "<refresh_token>" #Optional

response = NativeSocialApi.get_access_token_by_google_access_token(google_access_token, client_id, refresh_token)

 ```
 
  
  
 
#### Access Token using google JWT token for Native Mobile Login (GET)

This API is used to Get LoginRadius Access Token using google jwt id token for google native mobile login/registration.   

 
 

 ```

 id_token = "<id_token>" #Required

response = NativeSocialApi.get_access_token_by_google_j_w_t_access_token(id_token)

 ```
 
  
  
 
#### Access Token via Linkedin Token (GET)

The API is used to get LoginRadius access token by sending Linkedin's access token. It will be valid for the specific duration of time specified in the response.   

 
 

 ```

 ln_access_token = "<ln_access_token>" #Required

response = NativeSocialApi.get_access_token_by_linkedin_access_token(ln_access_token)

 ```
 
  
  
 
#### Access Token via Google AuthCode (GET)

This API is used to get LoginRadius access token by sending Google's AuthCode. It will be valid for the specific duration of time specified in the response.   

 
 

 ```

 google_authcode = "<google_authcode>" #Required

response = NativeSocialApi.get_access_token_by_google_auth_code(google_authcode)

 ```
 
  
  
 
 

### WebHook API


List of APIs in this Section:

* POST : [Webhook Subscribe](#webhook-subscribe-post)
* GET : [Webhook Subscribed URLs](#webhook-subscribed-urls-get)
* GET : [Webhook Test](#webhook-test-get)
* DELETE : [WebHook Unsubscribe](#webhook-unsubscribe-delete)



#### Webhook Subscribe (POST)

This API can be used to configure a WebHook on your LoginRadius site. Webhooks also work on subscribe and notification model, subscribe your hook and get a notification. Equivalent to RESThook but these provide security on basis of signature and RESThook work on unique URL. Following are the events that are allowed by LoginRadius to trigger a WebHook service call.   

 
 

 ```


 web_hook_subscribe_model ={ 
|Event|Allowed events: Login, Register, UpdateProfile, ResetPassword, ChangePassword, emailVerification, AddEmail, RemoveEmail, BlockAccount, DeleteAccount, SetUsername, AssignRoles, UnassignRoles, SetPassword, LinkAccount, UnlinkAccount, UpdatePhoneId, VerifyPhoneNumber, CreateCustomObject, UpdateCustomobject, DeleteCustomObject
|TargetUrl|URL where trigger will send data when it invoke  }  #Required

response = WebHookApi.web_hook_subscribe(web_hook_subscribe_model)

 ```
 
  
  
 
#### Webhook Subscribed URLs (GET)

This API is used to fetch all the subscribed URLs, for particular event   

 
 

 ```

 event = "<event>" #Required

response = WebHookApi.get_web_hook_subscribed_u_r_ls(event)

 ```
 
  
  
 
#### Webhook Test (GET)

This API can be used to test a subscribed WebHook.   

 
 

 ```


response = WebHookApi.webhook_test()

 ```
 
  
  
 
#### WebHook Unsubscribe (DELETE)

This API can be used to unsubscribe a WebHook configured on your LoginRadius site.   

 
 

 ```


 web_hook_subscribe_model ={ 
|Event|Allowed events: Login, Register, UpdateProfile, ResetPassword, ChangePassword, emailVerification, AddEmail, RemoveEmail, BlockAccount, DeleteAccount, SetUsername, AssignRoles, UnassignRoles, SetPassword, LinkAccount, UnlinkAccount, UpdatePhoneId, VerifyPhoneNumber, CreateCustomObject, UpdateCustomobject, DeleteCustomObject
|TargetUrl|URL where trigger will send data when it invoke  }  #Required

response = WebHookApi.web_hook_unsubscribe(web_hook_subscribe_model)

 ```



[Go Back to Home Page](/)
