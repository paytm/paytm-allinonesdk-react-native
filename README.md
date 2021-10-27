# paytm_allinone_react-native

This plugin exposes a single method to perform transaction

## Installation

```sh
npm install paytm_allinone_react-native --save
npx react-native link
```

## Usage

```js
    startTransaction(
        orderId: string,
        mid: string,
        txnToken: string,
        amount: string,
        callbackUrl: string,
        isStaging: boolean,
        restrictAppInvoke: boolean
    ): Promise<any>;
```

## Configuration

### Android

1. Add the below line to the ‘repositories’ section of your project-level build.gradle file as below

```sh
allprojects {
    repositories {
        google()
        .
        .
        maven {
            url "https://artifactory.paytm.in/libs-release-local"
        }
    }
}
```

### iOS

1: Open Podfile and Update Platform Version
Navigate to the ios folder and open Podfile. You can do this using the following code.
$ cd ios && open podfile.

2: Install Pods Using Cocoapods
Install pods after updating iOS platform. : pod install && cd .. 3. Add the following in ios project.
Open the projectName.workspace in ios folder.
Open Info.plist : Add LSApplicationQueriesSchemes. Change its type to Array. Create a new item in it and set its value as "paytm"

Go to Info tab -> URL Types : Add a new URL Type that you’ll be using as the callback from Paytm App (URL Scheme: "paytm"+"MID"). Example: paytmMid123

Open AppDelegate.m: Add following method before the end of the file ended by @end
Open AppDelegate.m and Import LinkingManager to the top of the file which has delegate methods for implementing and handling URLScheme(response from Paytm invoke will be received here in this method. Which in turns notifies the Plugin.)

#import <React/RCTLinkingManager.h>

- (BOOL)application:(UIApplication _)app openURL:(NSURL _)url
  options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> \*)options
  {

  NSString *urlString = url.absoluteString;
  NSDictionary *userInfo =
  [NSDictionary dictionaryWithObject:urlString forKey:@"appInvokeNotificationKey"];
  [[NSNotificationCenter defaultCenter] postNotificationName:
  @"appInvokeNotification" object:nil userInfo:userInfo];
  return [RCTLinkingManager application:app openURL:url options:options];
  }

## Usage

1. Import top-level package

```js
import AllInOneSDKManager from 'paytm_allinone_react-native';
```

2. Call the **startTransaction** api

```js
AllInOneSDKManager.startTransaction(
  orderId,
  mid,
  tranxToken,
  amount,
  callbackUrl,
  isStaging,
  appInvokeRestricted,
  urlScheme,
)
  .then((result) => {
    console.log('result', result);
    updateUI(result);
  })
  .catch((err) => {
    handleError(err);
  });
```

**For more details, please visit**

<code><a>https://developer.paytm.com/docs/all-in-one-sdk/</a></code>

## License

MIT
