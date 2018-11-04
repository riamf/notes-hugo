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

SPM was released with Swift 3.0 and is supported with this and all above Swift versions. If you come from iOS world then you will recognize it as similar to [Cocoapods](https://cocoapods.org/) or [Carthage](https://github.com/Carthage/Carthage), because is actually is doing exactly the same stuff. It is a simple dependency management tool that helps you keep track of, compile and link all external libraries that you use. What is really nice about SPM is that it works on Linux 💯. It is vastly used among desktop macOS apps or Linux apps written in Swift. But beside helping you to manage dependencies you can actually use it to create one, like a library you would like to publish for open source community or one that you are using internally in your company and would like to keep it maintained. If you want to know more about it I recommend checking one of the great tutorials from Ray Wenderlich sites written by Mikael Konutgan [An Introduction to the Swift Package Manager](https://www.raywenderlich.com/750-an-introduction-to-the-swift-package-manager) or going directly to SPM [documentation](https://swift.org/package-manager/). Here the only thing you have to know is that if you would like to use some external library in vapor app, you can use SPM for to do it and I will present it many times in next chapters.

# Vapor CLI

So to use Vapor you need to install it. On macOS I am using brew to manage most stuff that I am using so I recommend to just open Terminal and type in:
```
brew install vapor/tap/vapor
```
This will download and install Vapor Command Line Interface.
On Linux you just have to type this in console:
```
eval "$(curl -sL https://apt.vapor.sh)"
```
It will also tell you if your Linux version is supported if any issues appear.

Now that you have this CLI tool installed you can check Swift and Xcode compatibility by running in Terminal:
```
eval "$(curl -sL check.vapor.sh)"
```
it should tell you something like this:

![vapor check](/images/vapor_check.png)

As you can see I have a warning, this is because I recently updated macOS to Mojave and Xcode to version 10 that also updated my CLI tools and Swift to 4.2. These versions are not yet fully supported by Vapor team but I did not noticed any issues while playing around with Vapor and hopefully you won't see any.

Ok, now that we have all things ready, go into your working directory and create sample app by typing in Terminal:
```
vapor new sampleApp
```
This will make  new folder called `sampleApp`, if you `cd` inside you will see something like:

![vapor sample dir](/images/vapor_sample_dir.png)

This is a structure of a base Vapor template. It event contains some sample model and SQLite database configuration with implementation of base CRUD operations.

Now, lets Build and run this project.

If you are using macOS and want to use Xcode, please jump here [Using Xcode](#using-xcode)

For rest of us using command line, to build project type:
```
vapor build
```

This might take a while cause vapor will fetch all dependencies and then will build everything. After that just use `run` command:
```
vapor run
```

## Using Xcode

Vapor has a full support for running in Xcode, to make it happen though you need to generate Xcode project, but don't worry Vapor CLI will help you with that. Just `cd` into `sampleApp` directory and type:
```
vapor xcode -y
```
This command will generate and open Xcode project for you. Having Xcode support is great cause it gives you breakpoints and all LLDB commands just like in iOS/macOS apps 😍. From Xcode you can build and run Vapor app same as you would do with iOS or macOS app by using ⌘+r to build&run or ⌘+b to build ⌘+c to clean etc it all works the same. Sample vapor app comes with Two default schemas, one is for tests and one for building and running the app, be sure you have selected correct `Run` scheme:

![vapor scheme](/images/vapor_scheme.png)

# Where is my server?

By performing above  whether in Xcode or in Terminal you will start server and that will make your app accessible locally under port `8080` so if you go [here](http://localhost:8080/) you should see something like this:

![sample localhost](/images/vapor_sample_localhost.png)

Yeah, it works! Now you know how to start and run your project.

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
