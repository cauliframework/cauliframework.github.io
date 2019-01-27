---
layout: default
title: Home
---

# Cauli

Cauli is a network debugging framework featuring a plugin infrastructure to hook into selected request and responses as well as recording and displaying performed requests.

**Cauli is designed for developing purposes. Even if it doesn't use any private API we recommend against using it in a production environment.**

## Installation
### [CocoaPods](https://cocoapods.org)

Use the following in your Podfile.

```ruby
pod 'Cauli', '~> 1.0'
```

Then run `pod install`. 

### Carthage

[Carthage](https://github.com/Carthage/Carthage) is a non intrusive way to install Cauli to your project. It makes no changes to your Xcode project and workspace. Add the following to your Cartfile:

```swift
github "cauliframework/cauli"
```

### Setup

Add an `import Cauli` to your `AppDelegate` and call the `run` function on the shared instace in the `application(:, didFinishLaunchingWithOptions:)`.

```swift
import Cauli

public class AppDelegate: UIApplicationDelegate {
    public func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        Cauli.shared.run()
        // perform your usual application setup
        return true
    }
}
```

This will configure Cauli to hook into every request, setup a some plugins ([InspectorFloret](https://cauli.works/docs/Classes/InspectorFloret.html) and [MockFloret](https://cauli.works/docs/Classes/MockFloret.html)) and configures a shake gesture for the Cauli UI.