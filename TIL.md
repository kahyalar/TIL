# TIL - Today I Learned



## 11 January 2021

### #1 - Different ways to compare string in Swift

*URL: https://sarunw.com/posts/different-ways-to-compare-string-in-swift/*

```swift
let a = "a"
let capitalA = "A"

a.compare(capitalA, options: .caseInsensitive) 
// ComparisonResult.orderedSame

if a.compare(capitalA, options: .caseInsensitive) == .orderedSame {
    // a is equals to A
}

// or 

a.caseInsensitiveCompare(capitalA) 
// ComparisonResult.orderedSame
```

### #2 - Keep private information out of your logs with Swift

*URL: https://olegdreyman.medium.com/keep-private-information-out-of-your-logs-with-swift-bbd2fbcd9a40*

This wrapper doesn't do much: it simply makes sure that when your code is trying to `print` , `debugPrint` or `dump` a property, it will show `--redacted--` instead of an actual value. And since virtually any existing logging  system uses one of these functions, it will effectively remove any `@LoggingExcluded` property from our logs.

```swift
@propertyWrapper
struct LoggingExcluded<Value>: CustomStringConvertible, CustomDebugStringConvertible, CustomLeafReflectable {
    
    var wrappedValue: Value
    
    init(wrappedValue: Value) {
        self.wrappedValue = wrappedValue
    }
    
    var description: String {
        return "--redacted--"
    }
    
    var debugDescription: String {
        return "--redacted--"
    }
    
    var customMirror: Mirror {
        return Mirror(reflecting: "--redacted--")
    }
}
```

Example usage:

```swift
struct User {
    var identifier: String
    var handle: String
    @LoggingExcluded var name: String
    @LoggingExcluded var dateOfBirth: Date
    @LoggingExcluded var city: String
}

// print
User(identifier: "6EEBA88F-D397-4108-B3D9-2B60E5FB4477", handle: "swifty15", _name: --redacted--, _dateOfBirth: --redacted--, _city: --redacted--)
```

