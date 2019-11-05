---
title: "singletons Singletons Ambient Context"
categories:
  - software-engineering
tags:
  - swift
---
Here are three types of singletons. Can you spot the difference? How would you name the patterns? What are their benefits and risks?

```swift
class ExampleOne {
    static let shared = ExampleOne()

    private init {}
}
```

```swift
class ExampleTwo {
    static let shared = ExampleTwo()
}
```

```swift
class ExampleThree {
    static var sharedInstance = ExampleThree()
}
```

`ExampleOne`. This is by the book Singleton implementation. In Swift `static let` is a lazily loaded constant and preserves system resources at launch. The code makes sure that a class has only one instance. It provides a single point of access to it. As described in the Design Patterns book (GOF) by Gamma, Johnson, Vlissides, and Helm, we are making sure that the class itself keeps track of its sole instance. The code prohibits creating over one instance.

`ExampleTwo`. If you have encountered objects such as `URLSession.shared` or `UserDefaults.standard`, then those were the variations of Singleton known as a singleton with a lowercase ‘s’. We instantiate the class only one time in the whole life-cycle of the app. However, its API does not prohibit creating a new instance of the class.

`ExampleThree`. This is a mutable global shared state. We usually access it by a variable named `static sharedInstance`. It allows the access and mutation of that reference. This is also an example of Ambient Context anti-pattern. Apple uses it too. For instance `URLCache.shared` property is a static, globally accessible property that we can change.

## Singleton uses and problems they can bring to our codebase.

Critics consider the singleton to be an anti-pattern because it is frequently used in not beneficial scenarios. It sets up unnecessary restrictions in situations where a sole instance of a class is not required. It makes dependencies implicit, highly couples modules, prevents testability, introduces global state and forces thread-safety. Ambient Context is even worse. It allows changing the dependency causing temporal coupling, global state, runtime inconsistency and so on.

## When should we use the singleton pattern?

When we need precisely one instance of a class, and it must be accessible to clients from a well-known access point. Example: a class that logs messages to the console. Its public API is very simple and we don’t need over one instance or even re-create or mutate its reference in memory.

Examples of bad Singleton candidates are the objects not mandatory for a system to have only one instance of a type.

Singletons may damage our system design and testability if we access the concrete singleton instance directly. To escape the tight coupling we can use dependency inversion. By hiding the third-party dependency behind an interface we can keep the app modules agnostic about the implementation details.

## Sources

* [Singleton pattern - Wikipedia](https://en.wikipedia.org/wiki/Singleton_pattern)
* [Design Patterns: Elements of Reusable Object-Oriented Software](https://www.goodreads.com/book/show/85009.Design_Patterns)
