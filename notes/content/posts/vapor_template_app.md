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

It was created at Apple by people from iCloud team. You can check this [talk](https://www.youtube.com/watch?v=QJ3WG9kRLMo) from Norman Maurer one of the creators, it is a great talk. Under the hood Swift-NIO is an low level asynchronous, event driven network framework with non-blocking IO operations, it is based on [netty](https://netty.io/) but it is written in Swift 😍. If you are already familiar with `netty` you know that it is very good and there is no need for reinventing the wheel so guys from Apple decided to make it work, and they did, cheers for them 👏. Main thing to point here is that you will not use `Swift-NIO` at all, the only thing you need to know is that it is there somewhere under the hood making things easier for you. `Vapor 3` is already using it and provides high level API for users to easily interact with network events. It might look like I am underestimating the value of `Swift-NIO` here, but I want to write more about cool stuff from Vapor world and there is a loot!

# Swift Package Manager

# Route

# Leaf

So what is [Leaf](https://github.com/vapor/leaf)? It is a Vapor way of creating front-end applications, a very powerful templating language. So it is not like whole Angular or Rails that provides a lot of functionality out of the box but it has I would say a similar templating functionalities for creating web page templates. 

How to use it in Vapor project is very simple. With Swift Package Manager, cause this is a main dependency management system that is used in Vapor, most of this stuff is simple, you just need to add a Leaf package to the app like this:

{{< highlight swift "linenos=inline,linenostart=0" >}}
import PackageDescription

let package = Package(
    name: "vapor_base",
    dependencies: [
        .package(url: "https://github.com/vapor/vapor.git", from: "3.0.0"),
        .package(url: "https://github.com/vapor/leaf.git", from: "3.0.1")],
    targets: [
        .target(name: "App", dependencies: ["Vapor",
                                            "Leaf"]),
        .target(name: "Run", dependencies: ["App"]),
        .testTarget(name: "AppTests", dependencies: ["App"])])
{{< / highlight >}}

This will add Leaf package to your application, from this point you can use it to build sites.

Next thing that needs to be done in the app is render configuration, we need to tell the app that we now have leaf and we would prefer it to use new Renderer. To do this just we just need import Leaf framework in `configure.swift` file and add that line at the end of `configure` function:

{{< highlight swift "linenos=inline,linenostart=0" >}}
public func configure(_ config: inout Config, _ env: inout Environment, _ services: inout Services) throws {
    /* ... */
    config.prefer(LeafRenderer.self, for: ViewRenderer.self)
    /* ... */
}
{{< / highlight >}}

Now the only thing to do is prepare html templates. There is one default directory that is used by Leaf to look for web pages, it is called `Resources` and inside it a directory called `Views`, (`./Resources/Views`). Important thing is to create it in application root directory like this

![vapor resources views](/images/vapor_resources_views.png)

Now you are ready to create first templates. In `Xcode` just select `New File` and ftom Other types select Empty and call it `index.leaf`. This way Leaf will know that it should render this file with injected context. Now I will show you an example content for this kind of file:

{{< highlight html "linenos=inline,linenostart=0" >}}
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<title>Welcome vapor template</title>
</head>
<body>
    #if(name) {
        <h1>Welcome #(name)</h1>
    } else {
        <h1>Welcome</h1>
    }
</body>
</html>
{{< / highlight >}}

This file will be rendered as our index file. You can clearly see that it is just a simple html with some additional leaf tags. Here I used `#if` tag to check if variable `name` was passed into this template, if it was then I am using it to print welcome with `name` and if not then just a simple welcome.

I thing it is clear how this part works, there is a lot tags you can use in templates I recommend looking into (leaf documentation)[https://docs.vapor.codes/3.0/leaf/overview/].
What you might wonder now it how this context is passed into the page. This is done by some magic in router. Let me show you example index route and then I will explain it all step by step:

{{< highlight swift "linenos=inline,linenostart=0" >}}
struct IndexContext: Codable {
    let name: String
}

public func routes(_ router: Router) throws {

    router.get("") { (request) -> Future<View> in
        return try request.view().render("index", IndexContext(name: "You"))
    }
}
{{< / highlight >}}

Sample vapor app has this file called `routes.swift` that you already know about, There you can declare routes handled by the app. I decided that going under empty route(line 6) so basically going to our server address `http://localhost:8080` I want it to render my index template and that what is going on in line 7.
You can also see that I declared simple `struct` called `IndexContext`(line 0) that contain value `name` of type `String`. This is a page context that I am also passing to Leaf in line 7. If you look at leaf template above you can see that when I'm checking `name` variable it is actually a variable from this `IndexContext`, this structure values are passed down to the template and you don't have to do anything like `context.name` it is already there.
This is just a dummy example of how values can be passed into Leaf templates but I think you can already see the possibilities and power of Leaf. You can pass there information extracted from database, whole models and by creating templates, tell them how to render the View without duplicating code. Whole logic can be done in Context that is a Model for Leaf that is just a dummy view Layer when you think of it in standard web MVC context.

# Fluent
# Authentication
# Template App
## Creating App
## Adding dependencies
## Vapor update
## Example route
## User model
## Basic Auth
## Adding hidden route
### Covering user data
## Seeding database
## Deployment
## Summary
## External sources
