---
layout: post
title: Mock Network Requests
---

The Cauli Framework brings along a very mighty [Floret](https://cauli.works/docs/Protocols/Floret.html) that helps you to get up and running mocking all your network requests in no time. The [MockFloret](https://cauli.works/docs/Classes/MockFloret.html) helps recording all network requests which then can be included for your Unit Tests and specifically selected.

<!-- more -->

## Requirements

The `MockFloret` depends on `Cauli` so first make sure you have `Cauli` installed and setup as described in the [Getting Started](https://github.com/cauliframework/cauli#getting-started) guide. Cauli works by leveraging the [URL Loading System](https://developer.apple.com/documentation/foundation/url_loading_system) so it can mock requests executed by the [URLSession](https://developer.apple.com/documentation/foundation/urlsession).

## MockFloret Setup

To get the `MockFloret` running all you have to do is to create an instance of `MockFloret` and create your `Cauli` instance containing this Floret. 

```swift
let mockFloret = MockFloret(mode: .record)
let cauli = Cauli([mockFloret], configuration: Configuration.standard)
```

Since your application can use multiple `Cauli` instances, you can create a specific once just for mocking purposes and configure it to just work on specific requests, like matching just your host for example.

```swift
let recordSelector = RecordSelector { record in
    if let host = record.designatedRequest.url?.host {
        return host == "cauli.works"
    }
    return false
}
let configuration = Configuration(recordSelector: recordSelector, enableShakeGesture: true)
let mockFloret = MockFloret()
let mockingCauli = Cauli([mockFloret], configuration: configuration)
```

## Recording

The `MockFloret` works in two modes. One is the `.record` mode where it will record every request and response intercepted by `Cauli` and serializes it to the Document directory of the simulator. The exact path will be printed in the console once you create your `MockFloret` instance. Just search for "recording to".

```swift
let mockFloret = MockFloret(mode: .record)
```

## Automatic Mocking

Once you recorded all the requests you want to mock just copy the "MockFloret" folder from the printed out path to your project and make sure you tick "Create folder references" for this one.

![placeholder](/public/assets/mock-network-requests-mockfloret-folder.png "Large example image"){:width="292px"}

Make sure you change the mode to the `.mock` mode wich is the default mode of the `MockFloret`.

```swift
let mockFloret = MockFloret(mode: .mock(forced: true))
```

## Customized Mocking

The `.mock` mode of the `MockFloret` can be configured to force mocking, so all requests for which no response is found a 404 response will be returned. This is helpful for UI- and Unit-Testing purposes. You can disable force mocking, fo all those requests will be ignored and default URLSession loading takes over. 

```swift
let mockFloret = MockFloret(mode: .mock(forced: false))
```

For all cases where the automatic mocking doesn't work, a custom mapping can be defined. 

This mapping will catch all requests, where the path is `/api/sign_in`. It will search the `MockFloret/default/successful_sign_in` folder in the bundle for a valid response and return that to the application.

```swift
mockFloret.addMapping(forUrlPath: "/api/sign_in") { request, floret in
    return floret.resultForPath("default/successful_sign_in")
}
```

This mapping will catch all requets for the domain `cauli.works` and responds with a 404 not found response.

```swift
floret.addMapping(with: { request, floret -> Result<Response>? in
    guard request.url?.host == "cauli.works" else { return nil }
    return Result<Response>.notFound(for: request)
})
```

If a mapping returns nil, this mapping will be skippend and other mappings or  the automatic mapping is used in order to find a correct response.

## Further information

For more information please refer to the [documentation](/docs/). If found a bug please don't hesitate and open an issue on [github.com/cauliframework/cauli](https://github.com/cauliframework/cauli/issues/new).