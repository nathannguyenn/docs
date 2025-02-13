---
title: iOS Library
description: "A guide to implement LoginRadius SDK in iOS application."
summary: "A guide to implement LoginRadius SDK in iOS application"
path: "/references/sdk/ios-sdk"
---

# iOS SDK


> **Disclaimer**: This library is meant to help you with a quick implementation of the LoginRadius platform and also to serve as a reference point for the LoginRadius API. Keep in mind that it is an open source library, which means you are free to download and customize the library functions based on your specific application needs.

This document provides instructions to integrate the LoginRadius User Registration Service or Social Login in an iOS app. You can download the iOS SDK from [here](https://github.com/LoginRadius/ios-sdk).

## SDK Installation

We recommend using CocoaPods for installing the library in a project.

[CocoaPods](http://cocoapods.org/) is a dependency manager for Cocoa projects. You can install it with the following command:

```
$ gem install cocoapods
```

**Podfile**

Open a terminal window and navigate to the location of the Xcode project for your application. If you have not already created a Podfile for your application, create one now:

```
$ pod init
```

To integrate LoginRadiusSDK into your Xcode project using CocoaPods, specify it in your `Podfile`:

```
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '9.0'

target 'TargetName' do
pod 'LoginRadiusSDK', '~> 5.4.0'
end
```

Then, run the following command:

```
$ pod install
```

## Prerequisites
[OS X](http://www.apple.com/macos/sierra/), [Xcode](https://developer.apple.com/xcode/) and iOS 9 or higher.


## Initialize SDK

1. Create a new File `LoginRadius.plist` and add it to your App

2. Add the following entries to your `LoginRadius.plist`

|       Key       |  Type   |    Required                                                                     |
| --------------- | ------- | ------------------------------------------------------------------------------- |
|apiKey          |String  |Yes                                                                                |
|siteName        |String  |Yes                                                                                |
|verificationUrl |String  |Optional, (Default URL: `https://auth.lrcontent.com/mobile/verification/index.html`)  |
|useKeychain\*   |Boolean |Optional, No by default (it is used to maintin session, if set to false then user detail will be saved in user defaults)                                                            |
|customDomain    |String  |Optional, (Default URL: `https://api.loginradius.com/`)                               |


> **Note**: You can get API Key and Site/App Name from the [Configuration](https://dashboard.loginradius.com/configuration) section of your LoginRadius Dashboard.


3. Import the module in your source code.

```objectiveC
#import <LoginRadiusSDK/LoginRadius.h>
```

**Application is launched**

Initialize the SDK with your API key and Site name in your `AppDelegate.m`.
```objectiveC
//  AppDelegate.m

#import <LoginRadiusSDK/LoginRadius.h>

- (BOOL)application:(UIApplication _)application didFinishLaunchingWithOptions:(NSDictionary _)launchOptions {
  LoginRadiusSDK \* sdk = [LoginRadiusSDK instance];
  [sdk applicationLaunchedWithOptions:launchOptions];

      //Your code

      return YES;

  }
```
__Application to listen your URL__

You need to configure your Custom URL Scheme for this library to work.

1. In Xcode, right-click on your project's .plist file and select Open As -> Source Code.**Default plist is usually your Info.plist file**
2. Insert the following XML snippet into the body of your file just before the final element. Replace **{your-sitename}** with your LoginRadius Site Name. And then Replace **{your-app-bundle-identifier}** with your app's bundle identifier. If you don't know where your app bundle identifier is, see point 3.

```XML
<key>CFBundleURLTypes</key>
<array>
<dict>
 <key>CFBundleURLSchemes</key>
 <array>
   <string>{your-sitename}.{your-app-bundle-identifier}</string>
 </array>
</dict>
</array>
```

3. If you don't know where is your app bundle identifier, see below

![How to get bundle identifier in xcode](https://apidocs.lrcontent.com/images/Get-Bundle-Identifier_315859318e7c077974.95870376.png)

**Application is asked to open URL**

Call this to handle URL's for social login to work properly in your `AppDelegate.m`

```objectiveC
//  AppDelegate.m
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
    return [[LoginRadiusSDK sharedInstance] application:app
                                                openURL:url
                                      sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]
                                             annotation:options[UIApplicationOpenURLOptionsAnnotationKey]];
}
```


## Integrate Registration Service

Registration service supports email/password (traditional) registration and login methods.

Registration Service is done through the Authentication API.

Registration requires a parameter called SOTT. You can create the SOTT token by following this [doc](https://dashboard.loginradius.com/dashboard)

**Parameters and their Description:**

|Name|Description|Required|
|---|----|----|
|SOTT|Secure One-time Token which you can check information about SOTT [here](https://dashboard.loginradius.com/dashboard)|Yes for Registration.  You can generate a long term SOTT token from the Dashboard
|smstemplate|SMS template allows you to customize the formatting and text of SMS sent by users who share your content.|NO|
|emailTemplate|Email templates allow you to customize the formatting and text of emails sent by users who share your content. Templates can be text-only, or HTML and text, in which case the user's email client will determine which is displayed. |NO  Go To API Configuration -> Email Workflow to get the template names|

**Registration by Email:**

```objectiveC
NSDictionary *payload=@{ @"Email": @[ @{ @"Type": @"Primary", @"Value": @"test@gmail.com"}
                                    ],@"Password": @"password" };

[[AuthenticationAPI authInstance]  userRegistrationWithSott:@"<your sott here>"  payload:payload emailtemplate:nil smstemplate:nil preventVerificationEmail:false completionHandler:^(NSDictionary * _Nullable data, NSError * _Nullable error) {

    if (!error) {
        // Registration only registers the user. Call login to set the session
        NSLog(@"successfully reg %@", data);
    } else {
        NSLog(@"Error: %@", [error description]);
    }

}];
```

> Registration API will only create a user. To retrieve userProfile and access_token, please call Login API.

For all the possible payload fields, please check the Auth User Registration by Email 



## Integrate Email/Password (Traditional) Login

Following code can be used for implementation of email/password (traditional) login:

**Login by Email:**

Call this function to login the user by email.

```objectiveC
NSDictionary *parameter =  @{ @"Email":@"email",
                           @"Password":@"password",
                           @"securityanswer":@""
                           };

[[AuthenticationAPI authInstance] loginWithPayload:parameter loginurl:nil emailtemplate:nil smstemplate:nil g_recaptcha_response:nil completionHandler:^(NSDictionary * _Nullable data, NSError * _Nullable error) {
        if (!error) {
            NSLog(@"successfully logged in %@", data);
        } else {
            NSLog(@"Error: %@", [error description]);
        }
    }];
```



You can store access_token and userProfile after successful login in LRSession for a long time

```objectiveC
LRSession *session = [[LRSession alloc] initWithAccessToken:access_token userProfile:[[data mutableCopy] replaceNullWithBlank]];
```

After storing the value, you can get the userProfile and access_token from LRSession

```objectiveC
// Check for `isLoggedIn` on app launch to check if the user is logged in.

NSDictionary *profile =  [[[LoginRadiusSDK sharedInstance] session] userProfile];
NSString *access_token =  [[[LoginRadiusSDK sharedInstance] session] accessToken];
NSString *alreadyLoggedIn =  [[[LoginRadiusSDK sharedInstance] session] isLoggedIn];
```

## Integrate Forgot Password
Following code can used for implementation of forgot password feature:

```objectiveC
[[AuthenticationAPI authInstance] forgotPasswordWithEmail:@"<your email>" emailtemplate:nil completionHandler:^(NSDictionary * _Nullable data, NSError * _Nullable error) {
       if (!error) {
           NSLog(@"Successfully sent email");
       } else {
           NSLog(@"Error: %@", [error description]);
       }
    }];
```






## Integrate Social Login

Social Login can be done in two ways.

1. Web Social Login: This is done using `SFSafariViewController` or `UIWebView`.

  SFSafariViewController is the default choice for authentication. If it is not available i.e < iOS 9,social login falls back to UIWebView.

  > Note: Facebook and Google no longer allow OAuth requests in embedded browsers on iOS.
  For Google or Facebook social login to work you will need to use the Native Social Login implementation method.

2. Native Social Login

  Login is done natively, utilizing the respective provider SDK's.


### Web Social Login
Social Login with the given provider. Call this function in the view controller you have set up for the button views.

To integrate Web Social Login. Follow the steps

* Enable https://auth.lrcontent.com in your site list please add it under Deployment > Configuration > Apps for Social Login to work properly.

* Call `loginWithProvider:inController:completionHandler:` method with the appropriate params in your Application to start Web Social Login.

    
    Example:
```objectiveC
[[LoginRadiusSocialLoginManager sharedInstance] loginWithProvider:@"facebook" inController:self completionHandler:^(NSDictionary *data, NSError *error) {
        if (success) {
            NSLog(@"Successfully logged in");
        } else {
            NSLog(@"Error: %@", [error description]);
        }
    }];
```

### Native Social Login

**Facebook native login**

> For Native Facebook login to work, create and configure your Facebook app as per [facebook docs](https://developers.facebook.com/docs/facebook-login/ios).

You don't need to download and integrate the Facebook SDK with your project. It is distributed as a dependency with LoginRadius SDK. Just make sure your `Info.plist` looks like this

![![Facebook Native Login Configuration](https://apidocs.lrcontent.com/images/Screen-Shot-2017-04-18-at-12-21-30-PM_556158f5b7b278fd19.62604375.png "")](https://apidocs.lrcontent.com/images/fb_config_780858f5b8c422bed2.36763090.png "Facebook Configuration")

**If you are using our demo,** then go to our AppDelegate.m / AppDelegate.swift and set **useFacebookNative** to **true** to display our native facebook ui.

**If you are making your own app,** then proceed to add these lines of codes.

and you are calling
`application:openURL:sourceApplication:annotation` in your `AppDelegate.m`.

```objectiveC
//  AppDelegate.m

- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
    return [[LoginRadiusSDK sharedInstance] application:app
                                                openURL:url
                                      sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]
                                             annotation:options[UIApplicationOpenURLOptionsAnnotationKey]];
}
```

> Replace the values with your Facebook App ID and Display name from your App Settings page in [Facebook Developer portal](https://developers.facebook.com/)

Call the function to start Facebook Native Login.
```objectiveC
[[LoginRadiusSocialLoginManager sharedInstance] nativeFacebookLoginWithPermissions:@{@"facebookPermissions":@[@"public_profile"]}
         inController:self
      completionHandler: ^(NSDictionary *data, NSError *error){
        if(error){
            [self errorMessage:data error:error];
        }else {
            NSString *access_token= [data objectForKey:@"access_token"];
            NSString *refresh_token= [data objectForKey:@"refresh_token"];
            NSLog(@"LR Token%@",access_token);
            NSLog(@"LR Refresh Token%@",refresh_token);

        }
    }];
```

**Twitter Native Login**

As of iOS 11, Twitter Native Login is done through the TwitterKit Library. The TwitterKit library works to perform native login on previous iOS versions.

First, you need to create a Twitter App.
Get your Twitter Keys from [Twitter Dev Console](https://apps.twitter.com/). Go to your App Page and you will find the keys under __Keys And Access Tokens__ tab.

Then add "TwitterKit" Library to your iOS Project's `Podfile`.

```
pod 'TwitterKit'
```

You can follow the original guide over at Twitter's [documentation](https://dev.twitter.com/twitterkit/ios/overview)

Or follow these steps:

1. Instantiate the TwitterKit in `AppDelegate` during app launch

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

      [[TWTRTwitter sharedInstance] startWithConsumerKey:@"Your twitter consumer key" consumerSecret:@"Your twitter consumer SECRET key"];

      //Your other Library that needs instantiation, e.g. LoginRadiusSDK
  }
```
2. Add these in your `Info.plist`

```XML

<key>CFBundleURLTypes</key>
<array>
<dict>
<key>CFBundleURLSchemes</key>
<array>
<string>twitterkit-<consumerKey></string>
</array>
</dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
<string>twitter</string>
<string>twitterauth</string>
</array>

```

3. Linking your UIButton to Twitter Native Login link it with LoginRadius on success

```objectiveC
//This is the function call linked to a UIButton, you can add IBAction in here to link with the storyboard
  - (void)showNativeTwitterLogin {
      [[TWTRTwitter sharedInstance] logInWithCompletion:
      ^(TWTRSession * _Nullable session, NSError * _Nullable error) {
          if (session){
              [[LoginRadiusSocialLoginManager sharedInstance] convertTwitterTokenToLRToken:session.authToken twitterSecret:session.authTokenSecret inController:self completionHandler:^(NSDictionary * _Nullable data, NSError * _Nullable error) {
                  if (error){
                      //User cancelled or errored on LoginRadius Authentication Page
                  }else{
                      //Your Code after LoginRadius Authenticate Twitter Token
                  }
              }];
          } else if (error){
              //User cancelled or errored on Twitter Authentication Page
          }
      }];
  }
```

4. Add these code to handle Twitter's URL Redirection in `AppDelegate`
```objectiveC
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
      BOOL canOpen = NO;
      canOpen = (canOpen || [[TWTRTwitter sharedInstance] application:app openURL:url options:options]);
      canOpen = (canOpen || [[LoginRadiusSDK sharedInstance] application:app
                                                  openURL:url
                                        sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]
                                               annotation:options[UIApplicationOpenURLOptionsAnnotationKey]]);
      return canOpen;
  }
```


5. As the final step, handle Twitter log out

```objectiveC
- (void)logoutPressed:(id)sender {

      [self twitterLogout];
      [LoginRadiusSDK logout];

      //do your UI logout behaviour, e.g:
      [self.navigationController popViewControllerAnimated:YES];
  }

  - (void) twitterLogout
  {
      NSArray * twitterSessions;
      twitterSessions = [[[Twitter sharedInstance] sessionStore] existingUserSessions];
      if (twitterSessions){
          for (id session in twitterSessions){
              [[[TWTRTwitter sharedInstance] sessionStore] logOutUserID:[session userID]];
          }
      }
  }
```

We suggest you to OBFUSCATE YOUR KEYS.

**Google Native Login**

Google Native Login is done through Google SignIn Library since this is a static library and has problems when you are using CocoaPods with `uses_frameworks!`, you have to manually install the SDK.

Follow these steps:

1. For Google SignIn you would need a configuration file `GoogleServices-Info.plist`. You can generate one following the steps [here](https://console.developers.google.com/apis/credentials?project=_&refresh=1).

2. Drag the `GoogleService-Info.plist` file you just downloaded into the root of your Xcode project and add it to all targets.

3. Google SignIn requires a custom URL Scheme to be added to your project. Add it to your `Info.plist` file. Make sure your URL Schemes in URL Types look like this.

    ![enter image description here](https://apidocs.lrcontent.com/images/google_config_2754258f5bac5a74374.81071661.png "")
     > Replace `{your REVERSED_CLIENT_ID}` with value of `REVERSED_CLIENT_ID` from `GoogleServices-Info.plist` file.

4. Add Google Sign In by following the [documentation](https://developers.google.com/identity/sign-in/ios/sign-in)

5. If you are using our demo, go to our AppDelegate.m / AppDelegate.swift and set **useGoogleNative** to **true** to display our native google ui. Our demo already contain all the necessary code to perform native Google Sign in, you just have to uncomment any instance of ``/* Google Native SignIn <code block> */``

    If you are making your own app, proceed to add these lines of codes. You can also see our demo to see the native google sign in action!

6. Add Google SignIn Library to your `Podfile`. ``pod 'Google/SignIn'``

7. Now change your App Delegate's open URL to handle both google native sign in and our default logins

```objectiveC
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {

        BOOL canOpen = NO;

        canOpen = [[GIDSignIn sharedInstance] handleURL:url
        sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]
        annotation:options[UIApplicationOpenURLOptionsAnnotationKey]];

        canOpen = (canOpen || [[LoginRadiusSDK sharedInstance]
        application:app
        openURL:url
        sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]
        annotation:options[UIApplicationOpenURLOptionsAnnotationKey]]);

        return canOpen;
    }
```

8. You have to exchange the Google token with LoginRadius Token. Call the following function in the SignIn delegate method after successful sign in.

```objectiveC
- (void)signIn:(GIDSignIn *)signIn
     didSignInForUser:(GIDGoogleUser *)user
            withError:(NSError *)error {

    if (error != nil)
    {
        NSLog(@"Error: %@",error.localizedDescription);
    }
    else
    {
        NSString *googleToken = user.authentication.accessToken;
        NSString *refreshToken = user.authentication.refreshToken;
        NSString *clientID = user.authentication.clientID;
        UIViewController *currentVC = [(UINavigationController *)[[self window] rootViewController] topViewController];
        [[LoginRadiusSocialLoginManager sharedInstance] convertGoogleTokenToLRToken:googleToken google_refresh_token:refreshToken google_client_id:clientID inController:currentVC completionHandler:^(NSDictionary * _Nullable data, NSError * _Nullable error) {
            id safeData = (data) ? data : [NSNull null];
            id safeError = (error) ? error : [NSNull null];
            [[NSNotificationCenter defaultCenter] postNotificationName:@"userAuthenticatedFromNativeGoogle" object:nil userInfo:@{@"data":safeData,@"error":safeError}];
         }];
    }
}
```

9. As the final step, add the google native signOut on your logout button.

```objectiveC
- (IBAction)logoutPressed:(id)sender {

      [[GIDSignIn sharedInstance] signOut];

      [LoginRadiusSDK logout];
      [self.navigationController popViewControllerAnimated:YES];
    }
```
## Touch ID

LoginRadius SDK provides local authentication with TouchID, if available.

Call the function to start Authentication using TouchID.

```objectiveC
[[LRTouchIDAuth sharedInstance] localAuthenticationWithFallbackTitle:@"" completion:^(BOOL success, NSError *error) {
    if (success) {
        NSLog(@"successfully authenticated with touch id");
    } else {
        NSLog(@"Error: %@", [error description]);
    }
}];
```
This way on logout the access token and user profile are conserved, and the TouchID UI appears.
You can use the TouchID authentication to go to the profile page. You can only do this if the user logged in once using social login or email/password (traditional) login.

## Logout
Log out the user.

```
[LoginRadiusSDK logout];
```

## Access Token and User Profile
After successful login or social login LoginRadius access token and user profile can be accessed like this.

```
NSDictionary *profile = [[[LoginRadiusSDK sharedInstance] session] userProfile];
NSString *access_token = [[[LoginRadiusSDK sharedInstance] session] accessToken];
```

We added a small boolean function if you want to check whether the user is logged in or not.

```
BOOL isUserLoggedIn = [[[LoginRadiusSDK sharedInstance] session] isLoggedIn];
```



## LoginRadius API Showcase

This section helps you to explore various API methods of LoginRadius IOS SDK. They can be used to fulfill your identity based needs related to email/password (traditional) login, registration, social login and many more.

For more details check [API Reference Here](/#api)



[Go Back to Home Page](/)
