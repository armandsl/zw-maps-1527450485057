# zw-maps-1527450485057
This is a test repository , it will use with firebase web application.

# FirebaseUI for Web — Auth

FirebaseUI is an open-source JavaScript library for Web that provides simple,
customizable UI bindings on top of [Firebase](https://firebase.google.com) SDKs
to eliminate boilerplate code and promote best practices.

FirebaseUI Auth provides a drop-in auth solution that handles the UI flows for
signing in users with email addresses and passwords, phone numbers, Identity
Provider Sign In including Google, Facebook, GitHub, Twitter, Apple, Microsoft,
Yahoo, OpenID Connect (OIDC) providers and SAML providers. It is built on top of
[Firebase Auth](https://firebase.google.com/docs/auth).

The FirebaseUI component implements best practices for authentication on mobile
devices and websites, helping to sign-in and sign-up conversion for your app. It
also handles cases like account recovery and account linking that can be
security sensitive and error-prone to handle.

FirebaseUI fully supports all recent browsers. Signing in with federated
providers (Google, Facebook, Twitter, GitHub, Apple, Microsoft, Yahoo, OIDC,
SAML) is also supported in Cordova/Ionic environments. Additional non-browser
environments(React Native...) or Chrome extensions will be added once the
underlying Firebase core SDK supports them in a way that is compatible with
FirebaseUI.

## Table of Contents

1. [Zw Maps](#zw-maps-1527450485057)
2. [Installation](#installation)
3. [Usage instructions](#using-firebaseui-for-authentication)
4. [Configuration](#configuration)
5. [Customization](#customizing-firebaseui-for-authentication)
6. [Advanced](#advanced)
7. [Developer Setup](#developer-setup)
8. [IAP External Identities Support with FirebaseUI](#iap-external-identities-support-with-firebaseui)
9. [Cordova Setup](#cordova-setup)
10. [React DOM Setup](#react-dom-setup)
11. [Angular Setup](#angular-setup)
12. [Known issues](#known-issues)
13. [Deprecated APIs](#deprecated-apis)
14. [Release Notes](#release-notes)

## Zw Maps

Accessible here:
[https://zw-maps-1527450485057.firebaseapp.com](https://zw-maps-1527450485057.firebaseapp.com).

<p align="center">
  <img src="zw-maps-1527450485057/screenshot.png" width="300" title="Screenshot">
</p>

## Installation

### Option 1: CDN

You just need to include the following script and CSS file in the `<head>` tag
of your page, below the initialization snippet from the Firebase Console:

```html
<script src="https://www.gstatic.com/firebasejs/ui/4.7.1/firebase-ui-auth.js"></script>
<link type="text/css" rel="stylesheet" href="https://www.gstatic.com/firebasejs/ui/4.7.1/firebase-ui-auth.css" />
```

#### Localized Widget

Localized versions of the widget are available through the CDN. To use a
localized widget, load the localized JS library instead of the default library:

```html
<script src="https://www.gstatic.com/firebasejs/ui/4.7.1/firebase-ui-auth__{LANGUAGE_CODE}.js"></script>
<link type="text/css" rel="stylesheet" href="https://www.gstatic.com/firebasejs/ui/4.7.1/firebase-ui-auth.css" />
```

where `{LANGUAGE_CODE}` is replaced by the code of the language you want. For example, the English
version of the library is available at
`https://www.gstatic.com/firebasejs/ui/4.7.1/firebase-ui-auth__en.js`. The list of available
languages and their respective language codes can be found at [LANGUAGES.md](LANGUAGES.md).

Right-to-left languages also require the right-to-left version of the stylesheet, available at
`https://www.gstatic.com/firebasejs/ui/4.7.1/firebase-ui-auth-rtl.css`, instead of the default
stylesheet. The supported right-to-left languages are Arabic (ar), Farsi (fa), and Hebrew (iw).

### Option 2: npm Module

Install FirebaseUI and its peer-dependency Firebase via npm using the following
commands:

```bash
$ npm install firebase --save
$ npm install firebaseui --save
```

You can then `require` the following modules within your source files:

```javascript
var firebase = require('firebase');
var firebaseui = require('firebaseui');
// or using ES6 imports:
import * as firebaseui from 'firebaseui'
import 'firebaseui/dist/firebaseui.css'
```

Or include the required files in your HTML, if your HTTP Server serves the files
within `node_modules/`:

```html
<script src="node_modules/firebaseui/dist/firebaseui.js"></script>
<link type="text/css" rel="stylesheet" href="node_modules/firebaseui/dist/firebaseui.css" />
```

## Using FirebaseUI for Authentication

FirebaseUI includes the following flows:

1. Interaction with Identity Providers such as Google and Facebook
2. Phone number based authentication
3. Sign-up and sign-in with email accounts (email/password and email link)
4. Password reset
5. Prevention of account duplication (activated when
*"One account per email address"* setting is enabled in the
[Firebase console](https://console.firebase.google.com). This setting is enabled
by default.)
6. Integration with
[one-tap sign-up](https://developers.google.com/identity/one-tap/web/)
7. Ability to upgrade anonymous users through sign-in/sign-up.
8. Sign-in as a guest

### Configuring sign-in providers

To use FirebaseUI to authenticate users you first need to configure each
provider you want to use in their own developer app settings. Please read the
*Before you begin* section of Firebase Authentication at the following links:

- [Phone number](https://firebase.google.com/docs/auth/web/phone-auth)
- [Email and password](https://firebase.google.com/docs/auth/web/password-auth#before_you_begin)
- [Google](https://firebase.google.com/docs/auth/web/google-signin#before_you_begin)
- [Facebook](https://firebase.google.com/docs/auth/web/facebook-login#before_you_begin)
- [Twitter](https://firebase.google.com/docs/auth/web/twitter-login#before_you_begin)
- [Github](https://firebase.google.com/docs/auth/web/github-auth#before_you_begin)
- [Anonymous](https://firebase.google.com/docs/auth/web/anonymous-auth#before_you_begin)
- [Email link](https://firebase.google.com/docs/auth/web/email-link-auth#before_you_begin)
- [Apple](https://firebase.google.com/docs/auth/web/apple)
- [Microsoft](https://firebase.google.com/docs/auth/web/microsoft-oauth)
- [Yahoo](https://firebase.google.com/docs/auth/web/yahoo-oauth)

For [Google Cloud's Identity Platform (GCIP)](https://cloud.google.com/identity-cp/)
developers, you can also enable SAML and OIDC providers following the
instructions:

- [SAML](https://cloud.google.com/identity-cp/docs/how-to-enable-application-for-saml)
- [OIDC](https://cloud.google.com/identity-cp/docs/how-to-enable-application-for-oidc)

### Starting the sign-in flow

You first need to initialize your
[Firebase app](https://firebase.google.com/docs/web/setup). The
`firebase.auth.Auth` instance should be passed to the constructor of
`firebaseui.auth.AuthUI`. You can then call the `start` method with the CSS
selector that determines where to create the widget, and a configuration object.

The following example shows how to set up a sign-in screen with all supported
providers. Please refer to the [demo application in the examples folder](demo/)
for a more in-depth example, showcasing a Single Page Application mode.

> Firebase and FirebaseUI do not work when executed directly from a file (i.e.
> opening the file in your browser, not through a web server). Always run
> `firebase serve` (or your preferred local server) to test your app locally.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Zw Maps</title>
    <!-- *******************************************************************************************
       * TODO(DEVELOPER): Paste the initialization snippet from:
       * Firebase Console > Overview > Add Firebase to your web app. *
       ***************************************************************************************** -->
    <script src="https://www.gstatic.com/firebasejs/ui/4.7.1/firebase-ui-auth.js"></script>
    <link type="text/css" rel="stylesheet" href="https://www.gstatic.com/firebasejs/ui/4.7.1/firebase-ui-auth.css" />
    <script type="text/javascript">
      // FirebaseUI config.
      var uiConfig = {
        signInSuccessUrl: '<url-to-redirect-to-on-success>',
        signInOptions: [
          // Leave the lines as is for the providers you want to offer your users.
          firebase.auth.GoogleAuthProvider.PROVIDER_ID,
          firebase.auth.FacebookAuthProvider.PROVIDER_ID,
          firebase.auth.TwitterAuthProvider.PROVIDER_ID,
          firebase.auth.GithubAuthProvider.PROVIDER_ID,
          firebase.auth.EmailAuthProvider.PROVIDER_ID,
          firebase.auth.PhoneAuthProvider.PROVIDER_ID,
          firebaseui.auth.AnonymousAuthProvider.PROVIDER_ID
        ],
        // tosUrl and privacyPolicyUrl accept either url string or a callback
        // function.
        // Terms of service url/callback.
        tosUrl: '<your-tos-url>',
        // Privacy policy url/callback.
        privacyPolicyUrl: function() {
          window.location.assign('<your-privacy-policy-url>');
        }
      };

      // Initialize the FirebaseUI Widget using Firebase.
      var ui = new firebaseui.auth.AuthUI(firebase.auth());
      // The start method will wait until the DOM is loaded.
      ui.start('#firebaseui-auth-container', uiConfig);
    </script>
  </head>
  <body>
    <!-- The surrounding HTML is left untouched by FirebaseUI.
         Your app may use that space for branding, controls and other customizations.-->
    <h1>Welcome to Zw Maps</h1>
    <div id="firebaseui-auth-container"></div>
  </body>
</html>
```
