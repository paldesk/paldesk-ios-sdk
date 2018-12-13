[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)

# Paldesk

## Requirements

Paldesk SDK supports iOS 9+ and Swift 4.2.

## Installing

Currently only Carthage is supported.
Add ```github "paldesk/paldesk-ios-sdk"``` to your Cartfile and run ```carthage update --platform ios```

Drag the frameworks (Paldesk, ReSwift, SocketIO, Starscream, Kingfisher and Alamofire from *Carthage/Build*) to Linked Frameworks and Libraries section.

Add new Run Script phase in Build Phases and add:
```
/usr/local/bin/carthage copy-frameworks
```

Input files:
```
$(SRCROOT)/Carthage/Build/iOS/Kingfisher.framework
$(SRCROOT)/Carthage/Build/iOS/ReSwift.framework
$(SRCROOT)/Carthage/Build/iOS/Alamofire.framework
$(SRCROOT)/Carthage/Build/iOS/SocketIO.framework
$(SRCROOT)/Carthage/Build/iOS/Starscream.framework
$(SRCROOT)/Carthage/Build/iOS/Paldesk.framework

```
Output files:
```
$(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/Kingfisher.framework
$(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/ReSwift.framework
$(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/Alamofire.framework
$(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/SocketIO.framework
$(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/Starscream.framework
$(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/Paldesk.framework

```
Add NSPhotoLibraryUsageDescription to your Info.plist. It's needed for uploading images.

## Usage

Initialize Paldesk with your Api Key in app delegate:

```swift
// Swift
import Paldesk

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        
        PaldeskSDK.initialize(apiKey: "Your Api Key")

        return true
    }
```


```objc
// Objective-C
#import <Paldesk/Paldesk.h>

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.

    [PaldeskSDK initializeWithApiKey:@"Your Api Key"];

    return YES;
}

```

If you wish to enable logging, just add "loggingEnabled" parameter to initialize method:
```swift
// Swift
    PaldeskSDK.initialize(apiKey: "Your Api Key", loggingEnabled: true)
```

```objc
// Objective-C
    [PaldeskSDK initializeWithApiKey:@"Your Api Key" loggingEnabled:true];
```


## Starting conversation

To start conversation from your app, use:
```swift
// Swift
    PaldeskSDK.startConversation(viewController: self)
```

```objc
// Objective-C
    [PaldeskSDK startConversationWithViewController: self];
```
## Creating client

Depending on your Client authentification type settings from Webapp -> Administration -> iOS SDK (https://www.paldesk.com/), you can choose three options:

* No data required - client will appear as “visitor” - you are free to start conversation without setting information about your client
* You provide information about client through code - you can create client when your client becomes available to you with following method:
```swift
// Swift
    let params = [
        "email":"clientemail@mail.com",
        "externalId": "123456",
        "firstName": "First Name",
        "lastName": "Last Name"
    ]
    PaldeskSDK.createClient(params: params)
```

```objc
// Objective-C
    [PaldeskSDK createClientWithParams:@{
                                         @"email": @"test@fksdaodfkspo.com",
                                         @"externalId": @"testfasfasdf",
                                         @"firstName": @"First Name",
                                         @"lastName": @"Last Name"
                                         }];
```
  Email and ExternalId fields are mandatory and other fields are optional. ExternalId should be something unique from your side, preferably your client's id (or even email).
  If client information is not provided, your client will appear as “visitor". You can explicitly create "visitor" by calling:
```swift
// Swift
    PaldeskSDK.createAnonymousClient()
```

```objc
// Objective-C
    [PaldeskSDK createAnonymousClient];
```

* Client provides his information through form - your user will have to provide his information in registration form which will be shown on first start (prior to starting conversation).


## Clearing client

To clear (logout) current client, use:
```swift
// Swift
    PaldeskSDK.clear()
```

```objc
// Objective-C
    [PaldeskSDK clear];
```
  
Important: If you call create client method multiple times, it will matter only first time. If you wish to create different client, please call clear method first and then you are able to create client again.
