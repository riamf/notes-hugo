+++
title = "Vapor template app"
date = 2018-10-25T20:53:37+02:00
tags = ["Swift", "Vapor", "MySQL"]
categories = [""]
draft = false
+++

# Vapor

A long time ago in a galaxy far, far away...I used [Kitura](https://www.kitura.io/) to try out `Swift` on server. Simultaneously other frameworks were developed like [Perfect](https://perfect.org/) and [Vapor](https://vapor.codes/). At That time it was mostly alpha/beta versions or work in progress, it wasn't as pleasurable as using `Swift` on machines with Apple sign.

Recently I decided to go back to that idea and `Vapor` is what I choose to test. So what is `Vapor` and from where did it came.

Version `0.1.0` was released just a month after Apple open sourced Swift. Next there was 1.0 version in 2016 and 2.0 in May 2017, but that all is not important because one year later there was version 3.0 that changed everything.

So what was so important about this release was that whole framework got rebuild on top of `Swift-NIO`. It happened so quickly that even Apple acknowledge the speed with which it was adopted.

# Swift NIO

Where to start...well I can say short that it is a low level network communication framework for creating high level frameworks or applications, And that would be it. But let me explain some it step by step.

One of the most important things, [Swift-NIO](https://github.com/apple/swift-nio) is made and maintained by Apple. You can go on their github and check it out, it is really neat. I guess there is something in what Chris Lattner said about `Swift`, that the goal is a total world domination, it is on iOS, macOS, tvOS Linux and we can now build tensors with [Swift for tensorflow](https://github.com/tensorflow/swift), it is really cool, cause Swift is one of my favorite programming languages. 

It was created at Apple by people from iCloud team. You can check this [talk](https://www.youtube.com/watch?v=QJ3WG9kRLMo) from Norman Maurer one of the creators, it is a great talk. Under the hood Swift-NIO is an low level asynchronous, event driven network framework with non-blocking IO operations, it is based on [netty](https://netty.io/) but it is written in Swift 😍. If you are already familiar with `netty` you know that it is very good and there is no need for reinventing the wheel so guys from Apple decided to make it work, and they did, cheers for them 👏. Main thing to point here is that you will not use `Swift-NIO` at all, the only thing you need to know is that it is there somewhere under the hood making things easier for you. `Vapor 3` is already using it and provides high level API for users to easily interact with network events. I might underestimate the value of `Swift-NIO` here but I want to write more about cool stuff from Vapor world and there is a loot!

# Leaf
# Fluent
# Swift Package Manager
# Authentication
# Template App
## Creating App
## Adding dependencies
## `Vapor update`
## Example route
## User model
## Basic Auth
## Adding hidden route
### Covering user data
## Seeding database
## Deployment
## Summary

