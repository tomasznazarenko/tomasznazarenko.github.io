---
title: "On function signatures style"
excerpt_separator: "<!--more-->"
categories:
  - software-engineering
tags:
  - quality
  - team
---

Teams have to agree on coding style. Developers have discussed naming and styling functions on the Internet and in the books. I am sure you hold your preferences. Yet, having studied linguistics as a hobby for a short time, and just as an avid book reader, I offer you a perspective that may help you write more readable code, allowing your future self and your colleagues find and grasp your code quicker.

Keep in mind that this is still a preference. If I find any better solution, I will update the post.

We read code looking for concepts in it, and to reason easily about them. Reading lots of books, made our minds proficient in finding book-alike structured information. In code, we convey information by arranging it, like in the books.

The structure usually looks like this:

Heading
  Subheading
    Body

Therefore, what disrupts reading is an unusual text flow. For example:

```swift
func mixedHeadingWithSubheading(one: Int,
  two: int) {}

// or

func unuxualTextFlow(one: Int, 
                     two: Int) {}

```

Contrary, following the book-alike structure in arranging functions signatures, makes them easier to read. Like this:

```swift
func inlineParametersLikeAHeading(one: Int, two: Int) { }

// or

func alignParametersLikeAHeadingAndSubheading(
  one: Int, two: Int) { }
```

Nothing fancy at the end. Just a reminder, that we spend a lot of time reading code, and structuring it well pays off. Your future self or your colleagues will thank you for that.