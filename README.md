#cordova-plugin-segment

Cordova plugin for Segment Analytics.

Supports Segment's iOS and Android SDKs.

##Usage
Implements (mostly) the same API interface on `window.analytics` as [Analytics.js][].

##Segment Write Keys
In config.xml, you can put the following preferences:
* \<preference name="analytics_write_key" value="{Segment write key}" />
* \<preference name="analytics_debug_write_key" value="{Segment write key}" />

##iOS Setup
This plugin uses cordova-plugin-cocoapods-support to automatically bundle in the Segment iOS SDK through CocaoPods.

The only caveat is that you will need to run the project from AppName.xcworkspace instead of AppName.xcodeproj (this is a limitation introduced by CocoaPods itself).

Also for this reason, if you are using Ionic and its command line tools, `ionic build` and `ionic run` will cause the archive build to fail. You will need to manually run the project in Xcode from AppName.xcworkspace.

You might also want to consult official Segment documentation [iOS Quickstart][].

##iOS Integrations Setup
This plugin only supports the AppboySegment Integration. To enable it, add the following piece of code to
AppDelegate.m

````

#import "AppDelegate.h"
#import "MainViewController.h"

#import <Analytics/SEGAnalytics.h>
#import "SEGAppboyIntegrationFactory.h"

@implementation AppDelegate

- (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
{
    self.viewController = [[MainViewController alloc] init];

    [[SEGAppboyIntegrationFactory instance] saveLaunchOptions:launchOptions];

    return [super application:application didFinishLaunchingWithOptions:launchOptions];
}

- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
  [[SEGAnalytics sharedAnalytics] registeredForRemoteNotificationsWithDeviceToken:deviceToken];
}

// iOS 8+ only
- (void)application:(UIApplication *)application didRegisterUserNotificationSettings:(UIUserNotificationSettings *)notificationSettings
{
  //register to receive notifications
  [application registerForRemoteNotifications];
}

- (void)application:(UIApplication *)application
  didReceiveRemoteNotification:(NSDictionary *)userInfo
  fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler
{
  [[SEGAnalytics sharedAnalytics] receivedRemoteNotification:userInfo];
}

@end

````

##Android Integrations Setup
Use Gradle:
By default, the plugin builds with the `analytics-core` SDK for Android.
To add more your custom integrations, create a `build-extras.gradle` file in your Android platform root directory and add your segment integration dependencies. See the [Android Custom Build Docs][] for examples.

[Analytics.js]: https://segment.io/docs/libraries/analytics.js
[iOS Quickstart]: https://segment.com/docs/libraries/ios/quickstart/
[Android Custom Build Docs]: https://segment.com/docs/libraries/android/#custom-builds
