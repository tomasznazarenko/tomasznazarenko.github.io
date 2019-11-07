---
title: "SwiftUI notes"
categories:
  - frameworks
tags:
  - swiftUI
---

I am updating this post on regular basis while studying SwiftUI.

## Terms

* modifier: modifiers are regular methods with one small difference: they always return a new instance of whatever you use them on.
* property wrapper:  a special attribute we can place before our properties, example: `@State`
* two-way binding: tells Swift that it should read the value of the property but also write it back as any changes happen. We bind a view that it shows the value of our property, but we also bind it so that any changes to the view also update the property. In Swift, we mark these two-way bindings with `$`.

## Tidbits

* Limit of 10 children inside a parent actually everywhere in SwiftUI. The limit is applied to prevent overloading UI prototyping performance. Use `Group` to avoid the limitation. `ForEach` doesn’t get hit by the 10-view limit.
* SwiftUI destroys and recreates structs frequently, so keeping them small and simple structs is important for performance.
* Views are a function of their state – you can show something if it reflects a value stored in your program. Everything the user can see is just the visible representation of the structs and properties in our code.

`Option+Cmd+P` shortcuts does the same as clicking Resume in the preview. Previews use an Xcode feature called `the canvas`, which is usually visible directly to the right of your code. You can customize the preview code if you want, and they will only affect the way the canvas shows your layouts – it won’t change the actual app that gets run.

## References

* [Apple SwiftUI Tutorials](https://developer.apple.com/tutorials/swiftui/tutorials)
* [SwiftUI Apple Developer Documentation](https://developer.apple.com/documentation/swiftui)
* [Fucking SwiftUI](https://fuckingswiftui.com)
* [Hacking with Swift Tutorial](https://www.hackingwithswift.com/books/ios-swiftui)

## Basic structure

`ContentView.swift` contains the initial user interface (UI) for your program.

`SceneDelegate.swift` contains code for launching one window in your app. This doesn’t do much on iPhone, but on iPad – where users can have multiple instances of your app open at the same time – this is important.

In Project Navigator the yellow group `Preview Content` , with `Preview Assets.xcassets` inside is another asset catalog, this time specifically for example images you want to use when you’re designing your user interfaces, to give you an idea of how they might look when the program is running.

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        Text("Hello World")
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

`View` comes from SwiftUI, and is the basic protocol that must be adopted by anything you want to draw on the screen – all text, buttons, images, and more are all views, including your own layouts that combine other views.

`some View` means it will return something that conforms to the `View` protocol, but that extra `some` keyword adds an important restriction: it must always be the *same* kind of view being returned – you can’t sometimes return one type of thing and other times return a different type of thing. it means “one specific sort of view must be sent back from this property.”

`ContentView_Previews` struct, which conforms to the `PreviewProvider` protocol. This piece of code won’t actually form part of your final app that goes to the App Store, but is instead specifically for Xcode to use so it can show a preview of your UI design alongside your code.

## Forms

`Form` is a container for grouping controls used for data entry, such as in settings or inspectors. Forms are scrolling lists of controls like text and images text fields, toggle switches, buttons, and more.

```swift
struct ContentView: View {
    var body: some View {
        Form {
            Group {
                Text("Hello World")
                Text("Hello World")
                Text("Hello World")
                Text("Hello World")
                Text("Hello World")
                Text("Hello World")
            }
            Group {
                Text("Hello World")
                Text("Hello World")
                Text("Hello World")
                Text("Hello World")
                Text("Hello World")
                Text("Hello World")
            }
        }
    }
}
```

In fact, you can have as many things inside a form as you want, although if you intend to add more than 10 SwiftUI requires that you place things in `Group`s to avoid problems.

`Group` is an affordance for grouping view content. Groups don’t actually change the way your user interface looks, they just let us work around SwiftUI’s limitation of ten child views inside a parent.

```swift
Form {
    Section {
        Text("Hello World")
    }

    Section {
        Text("Hello World")
        Text("Hello World")
    }
}
```

If you *want* your form to look different when split its items into chunks, you should use the `Section` view instead.

`Section` is an affordance for creating hierarchical view content. This splits your form into discrete visual groups, just like the Settings app does.

## NavigationView

`NavigationView` is a view for presenting a stack of views representing a visible path in a navigation hierarchy.

```swift
NavigationView {
    Form {
        Section {
            Text("Hello World")
        }
    }
    .navigationBarTitle(Text("SwiftUI"))
}
```

## State handling

```swift
struct ContentView: View {
    @State private var tapCount = 0

    var body: some View {
        Button("TapCount: \(tapCount)") {
            self.tapCount += 1
        }
    }
}
```

You can use `@State` property wrapper to hold a state in a struct. `@State` is specifically designed for simple properties that are stored in one view. As a result, Apple recommends we add **private** access control to those properties.

There are several ways of storing program state in SwiftUI, and you’ll learn all of them.

### Binding state to user interface

The problem is that Swift differentiates between “show the value of this property here” and “show the value of this property here, *but write any changes back to the property*.”

```swift
struct ContentView: View {
    @State private var name = ""

    var body: some View {
        Form {
            TextField("Enter your name", text: $name)
            Text("Your name is \(name)")
        }
    }
}
```

In the case of our text field, Swift needs to make sure whatever is in the text is also in the name property, so that it can fulfill its promise that our views are a function of their state – that everything the user can see is just the visible representation of the structs and properties in our code. This is what’s called a *two-way binding*.

## Creating views in a loop with `ForEach`

`ForEach`: a structure that computes views on demand from an underlying collection of of identified data This can loop over arrays and ranges, creating as many views as needed.

```swift
struct ContentView: View {
    let options = ["Option1", "Option2", "Option3"]
    @State private var selectedOption = "Option1"

    var body: some View {
        Picker("Select your option", selection: $selectedOption) {
            ForEach(0 ..< options.count) {
                Text(self.options[$0])
            }
        }
    }
}
```

## Lists

Lists work with identifiable data. You can make your data identifiable in one of two ways:

* by passing along with your data a key path to a property that uniquely identifies each element, or
* by making your data type conform to the Identifiable protocol.
