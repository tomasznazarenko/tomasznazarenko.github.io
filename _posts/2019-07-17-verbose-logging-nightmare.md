---
title: "Verbose logging nightmare"
excerpt_separator: "<!--more-->"
categories:
  - software-engineering
tags:
  - logging
---

Verbose logging is as useless as having none. I was in a mobile app project where logs littered the Xcode console. To sift through the logs required extraordinary focus. Application life cycle events and taps unleashed print-out hell. The console showed server responses mixed with Xcode warnings. Various methods spat custom messages. Many were vague old debugging remnants.

An app with a dirty log might be a sign of trouble ahead. How to know if something goes wrong in the development or production environment, and how to deal with it? Maybe our colleague at the back end monitors the system or we do it by ourselves. Either way, the logs are probably part of the monitoring.

You can use external logging tools or opt for Apple’s Unified Logging Framework. A good practice is abstracting the logging to a different module and hiding it behind an interface. A client may require changing the tool or adding new ones.

If a security policy disallows external tools or storing logs on a device, ask if you can send the logs to the back end. If you were to store all the logs in the memory, you could have depleted it. But you can consider a ring buffer to keep a few logs and send them in a batch at a planned request.

A cluttered log may show that your app will be hard to control once it reaches production.  Lend yourself or your team a hand and make sure to:

* Categorise log messages.
* Logging to the error category is important enough that you are willing to have someone call you about it.
* Be mindful about writing informative error messages.
* A noise-free error log becomes a first sign that your system is reasonably healthy, or serves as an early warning if it’s not.
* Have about one info log message for every significant application event.
* Delete obsolete logs.

## References

* [Ring buffer](https://en.wikipedia.org/wiki/Circular_buffer)
