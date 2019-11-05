---
title: "Improve build times in Swift with Xcode"
excerpt_separator: "<!--more-->"
categories:
  - software-engineering
tags:
  - xcode
  - tools
---

While sipping refreshment, I glanced at the activity view bar as Xcode was building project. I saw that until compilation  finishes I will gulp another glass. Was the size of the code base stretching the compiler limits? Why the Xcode got stuck at indexing?

As I was coding a new feature, I needed fast feedback from the compiler. With the subsequent builds, seconds added up to minutes. The sluggishness was derailing my workflow.

Quick builds guarantee we use our time and money efficiently, they prevent distractions and make the work a pleasure. Test Driven Development requires quick builds. Rapid builds save time and sanity when running tests again and again. I imagined compilation times not in minutes but in seconds. How would that speed up my work and increase our team response to bugs?

## Profiling

Xcode gives a way to find code lagging the compiler. Here's how profile your app.

In project `Build Settings` under `Swift-Compiler - Custom Flags`, `Other Swift Flags` add:

* `-Xfrontend -warn-long-expression-type-checking=300` ([apple/swift GitHub](https://github.com/apple/swift/pull/10214))
* `-Xfrontend -warn-long-function-bodies=300` ([apple/swift GitHub](https://github.com/apple/swift/commit/18c75928639acf0ccf0e1fb6729eea75bc09cbd5))
* `-Xfrontend -debug-time-function-bodies`

Experiment with the `300` argument. If compiler goes beyond the specified millisecond limit, Xcode will warn you and show you the offending place.

* Use Build Time Analyzer. It is a macOS app that shows you a break down of Swift build times. [Build Time Analyzer for Swift](https://github.com/RobertGummesson/BuildTimeAnalyzer-for-Xcode)

## Improving

How to improve build times? The easiest way:

* Use explicit types for complex properties. You save the time for the compiler to figure out the type. And you save your colleagues time on figuring out the type of the property.
* Provide types in complex closures.

* Don't use `+` to concatenate a strings.
* Precompute. Never do computations directly from the if-else conditions.
* Dependencies within a module are per-file. Dependencies across targets are for the entire target.

* Incremental Builds are file-based. Unrelated changes outside function bodies can still result in rebuilding. If you change the function bodies then other files won't be recompiled, as changes in function bodies do not affect the file's **interface**.
* Limit your objective-C/Swift **interface**. Keep your generated header minimal. User `private` when possible:
    * Make `IBOutlet private` and `IBAction private`.
    * Make methods exposed to Objective-C `@objc private`. Or you can switch to block based APIs, which may additionally cleanup your code.

Depending on your project and your needs, your can do more. Be sure to watch:

* [Building Faster in Xcode - WWDC 2018 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2018/408)
* [Behind the Scenes of the Xcode Build Process - WWDC 2018 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2018/415)

## Sources:

* [Building Faster in Xcode - WWDC 2018 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2018/408)
* [Behind the Scenes of the Xcode Build Process - WWDC 2018 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2018/415)
* [Speed up Swift compile time – Hacker Noon](https://hackernoon.com/speed-up-swift-compile-time-6f62d86f85e6)
* [Measuring compile times in Xcode 9](https://www.jessesquires.com/blog/measuring-compile-times-xcode9/)
* [Improving Your Build Time in Xcode 10 · Patrick Balestra](https://patrickbalestra.com/blog/2018/08/27/improving-your-build-time-in-xcode-10.html)
* [Optimizing Swift build time in Xcode 10](https://shashikantjagtap.net/wwdc18-modern-tips-for-optimising-swift-build-time-in-xcode-10/)
* [How to make Swift compile faster – Hacking with Swift](https://www.hackingwithswift.com/articles/11/how-to-make-swift-compile-faster)
* [Profiling your Swift compilation times · Bryan Irace](https://irace.me/swift-profiling)