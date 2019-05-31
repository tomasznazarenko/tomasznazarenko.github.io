---
title: "Don't Commit in Xcode Test Driven Development"
categories:
  - tools
tags:
  - xcode
  - tdd
---

Executing Git `commit` command is one of the sometimes overlooked Xcode features. Xcode by default associates this command with a shortcut `cmd + C`. Hitting that key combination displays a new window. In the middle of the window, there is an area between two source panes where Xcode shows numbered changes with clickable indicator. Clicking it allows to `Discard Change`. Yet, even more interesting, is the second choice `Don’t Commit`. It allows to leave out a change from a commit.

Where it shines is during test driven development, enabling quick commits after each incremental change in the tests or the production code. You can exclude commented code or unfinished ideas, and focus on committing just the code you want now to keep in the repository. All this without switch back and forth to a terminal or source control tool of your choice.

## Sources:
* [Source Control Workflows in Xcode - WWDC 2018 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2018/418/)