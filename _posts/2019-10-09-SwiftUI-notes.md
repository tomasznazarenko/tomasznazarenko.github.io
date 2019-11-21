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
* binding. A binding acts as a reference to a mutable state. It’s a value and a way to change that value.
* two-way binding: tells Swift that it should read the value of the property but also write it back as any changes happen. We bind a view that it shows the value of our property, but we also bind it so that any changes to the view also update the property. In Swift, we mark these two-way bindings with `$`.
* state. State is a value, or a set of values, that can change over time, and that affects a view’s behavior, content, or layout. You use a property with the `@State` attribute to add state to a view.
* observable object. A observable object is a custom object for your data that can be bound to a view from storage in SwiftUI’s environment. SwiftUI watches for any changes to observable objects that could affect a view, and displays the correct version of the view after a change.
* The @EnvironmentObject attribute. You use this attribute in views that are lower down in the view hierarchy to receive data from views that are higher up.
* The `environmentObject(_:)` modifier. You apply this modifier so that views further down in the view hierarchy can read data objects passed down through the environment.

## Tidbits

* Limit of 10 children inside a parent actually everywhere in SwiftUI. The limit is applied to prevent overloading UI prototyping performance. Use `Group` to avoid the limitation. `ForEach` doesn’t get hit by the 10-view limit.
* SwiftUI destroys and recreates structs frequently, so keeping them small and simple structs is important for performance.
* Views are a function of their state – you can show something if it reflects a value stored in your program. Everything the user can see is just the visible representation of the structs and properties in our code.
* You can pin a preview to the canvas when you’re developing and refining an animation. It will keep a particular preview open while you switch between different files in Xcode. If you don’t pin a preview, the canvas switches to display previews in the file you just opened.
* To debug views and to make `print()` calls work, you should first right-click on the play button in the preview canvas and choose “Debug Preview”.
* SwiftUI content views must return precisely one `View` we want to show. When we want more than one view on screen at a time we need to tell SwiftUI how to arrange them.

## Errors

* `Option+Cmd+P` shortcuts does the same as clicking Resume in the preview. Previews use an Xcode feature called "the canvas", which is usually visible directly to the right of your code. You can customize the preview code if you want, and they will only affect the way the canvas shows your layouts – it won’t change the actual app that gets run.
* The errors shown by SwiftUI can mislead you. Running the app in the simulator helps to unveil the cuase. Oftentimes you can ignore Xcode errors, as project builds and runs fine.

* Swift UI Error: `Unable to infer complex closure return type; add explicit type to disambiguate` can sometimes be solved by adding explicit closure return type. Example:

```swift
struct LandmarkList: View {
    var body: some View {
        NavigationView {
            List(landmarkData) { (landmark) -> NavigationLink<LandmarkRow, LandmarkDetail> in
                NavigationLink(destination: LandmarkDetail(landmark: landmark)) {
                    LandmarkRow(landmark: landmark)
                }
            }
            .navigationBarTitle(Text("Landmarks"))
        }
    }
}
```

## References

* [SwiftUI Essentials](https://developer.apple.com/videos/play/wwdc2019/216)
* [Apple SwiftUI Tutorials](https://developer.apple.com/tutorials/swiftui/tutorials)
* [SwiftUI Apple Developer Documentation](https://developer.apple.com/documentation/swiftui)
* [Fucking SwiftUI](https://fuckingswiftui.com)
* [Hacking with Swift Tutorial](https://www.hackingwithswift.com/books/ios-swiftui)

## Todo

[ ] How to scroll `TextField`s up when keyboard is shown so that textfields are not obscured. [StackOverflow-Sample-1](https://stackoverflow.com/questions/56716311/how-to-show-complete-list-when-keyboard-is-showing-up-in-swiftui)

## Arranging views

You can use stacks: Horizontal (HStack), vertical (VStack) and depth-based (ZStack) that places child views so they overlap.

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

You can have as many things inside a form as you want, although if you intend to add more than 10 SwiftUI requires that you place things in `Group`s to avoid problems.

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

## Bindings

### `@State`

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

There are several ways of storing program state in SwiftUI.

### Two way binding

When SwiftUI sees a property marked with this attribute, it automatically creates and manages persistent state behind the scenes and then exposes the value of that state through this property.
 If we just want to read or write to the data in our state, it's really easy. We can just read or write to a property directly.

 However, for example stepper also needs to be able to edit the state when its buttons are tapped. And we use this dollar sign prefix to indicate that we should pass a binding instead of just passing a read-only value.

 A binding is a kind of managed reference that allows one view to edit the state of another view.

Swift differentiates between “show the value of this property here” and “show the value of this property here, *but write any changes back to the property*.”  This is what’s called a *two-way binding*.

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

## Picker

```swift
Form {
    Section(header: Text("Avocado Toast")) {
        Picker(selection: $order.spread, label: Text("Spread")) {
            ForEach(Spread.allCases) { spread in
                Text(spread.name).tag(spread)
            }
        }
    }
}
```

## Lists

Lists work with identifiable data. You can make your data identifiable in one of two ways:

* by passing along with your data a key path to a property that uniquely identifies each element
* by making your data type conform to the Identifiable protocol.

Q: Which type do you use to make rows of a List tappable to navigate to another view?
A: `NavigationLink` - provide the destination view and the content of a row when you declare a NavigationLink.

To combine static and dynamic views in a list, or to combine two or more different groups of dynamic views, use the ForEach type instead of passing your collection of data to List.

## Drawing Paths and Shapes

You use GeometryReader to dynamically draw, position, and size views instead of hard-coding numbers that might not be correct when you reuse a view somewhere else in your app, or on a different-sized display. GeometryReader dynamically reports size and position information about the parent view and the device, and updates whenever the size changes; for example, when the user rotates their iPhone.

ZStack overlays views on top of each other.

## Animating Views and Transitions

Q: How do you prevent an animation from applying to certain modifiers in a sequence of modifiers?

Pass nil to the animation(_:) modifier.

```swift
Image(systemName: "chevron.right.circle")
    .imageScale(.large)
    .rotationEffect(.degrees(showDetail ? 90 : 0))
    .animation(nil)
    .scaleEffect(showDetail ? 1.5 : 1)
    .padding()
    .animation(.spring())
```

You can animate rotations that you create using the rotationEffect(_:) modifier.

Q: What’s a quick way to test how an animation behaves during interruptions like state changes?

Adjust the duration of the animation so that it runs long enough that you can observe and tune its fine details.
Making animations take longer is a quick and easily reversible change that’s effective for iterating on animations.

## View Builders

You can see the content parameter is defined as a closure but marked with the `@ViewBuilder` attribute.
The Swift Compiler knows how to translate a closure marked by this attribute into a new closure that returns a single view representing all of the contents within for example our stack.

## Modifiers

 You should try to push your conditions into your modifiers as much as possible. So instead if-ology try to see if you can use a modifier.
 Because that will help SwiftUI detect those changes and give you better animations.

## ForEach

`ForEach` takes a collection of data and a ViewBuilder that maps each data item into its own view. But unlike `List`, `ForEach` doesn't add any visual effects of its own. Instead, it just adds its own contents to its container.

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

`ForEach` operates on collections the same way as the list, which means you can use it anywhere you can use a child view, such as in stacks, lists, groups, and more. When the elements of your data are simple value types — like the strings you’re using here — you can use \.self as key path to the identifier.

Place a `ForEach` instance inside a List or other container type to create a dynamic list.

## If statements

In SwiftUI blocks, you use `if` statements to conditionally include views.
