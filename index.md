---
layout: default
title: Home
---

# Cauli

Cauli is a network debugging framework featuring a plugin infrastructure to hook into selected request and responses as well as recording and displaying performed requests. It provides a wide range of possibilities. For example from [inspecting](https://cauli.works/docs/florets.html#InspectorFloret) network traffic to [mock](https://cauli.works/docs/florets.html#MockFloret) UnitTests. Missing something fancy? How about [writing your own Plugin](https://cauli.works/docs/writing-your-own-plugin.html).

## Features

🌏 Hooks into the [URL Loading System](https://cauli.works/docs/frequently-asked-questions.html)  
🧩 [Existing](https://cauli.works/docs/florets.html) set of Plugins (Florets)  
🔧 [Extensible](https://cauli.works/docs/writing-your-own-plugin.html) Plugin Infrastructure

## Getting Started

### Installation
#### [CocoaPods](https://cocoapods.org)

Use the following in your Podfile.

```ruby
pod 'Cauli', git: 'https://github.com/cauliframework/cauli.git', branch: 'develop'
```

Then run `pod install`.

#### Carthage

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

This will configure Cauli to hook into every request, setup the core florets (plugins) ([InspectorFloret](https://cauli.works/docs/Classes/InspectorFloret.html)) and configures a shake gesture for the Cauli UI.
