# About the Platform
 DeJoy Mini Apps is a technology created by the developers of the famous messenger DeJoy. Its main purpose is to provide developers with a more flexible communication channel with  users.

It may seem unclear, but Mini Apps are not self-served services. The first thing to note is, technically, this technology is just an add-on for such already-known DeJoy  functionality as DeJoy Bots. So, currently, creating a Mini App without creating a DeJoy Bot is not possible.

The platform offers a high variety of available methods to communicate with the DeJoy application to make your web applications look more native, allow them to simulate native application's behavior, and finally, to mimic native applications.

# Required Technologies
Before starting to create an application on the Mini Apps platform, it is important to know what Mini Apps are from a technical standpoint. This will lead the developer to the selection of language and technology.

Internally, Mini Apps are typical web applications, which are displayed in WebView. In other words, they are just a set of static files (mostly .js, .css, and .html). So, to create Mini App, it is enough to learn standard front-end development technologies, such as:

 - JavaScript
 - CSS
 - HTML

Really simple, isn't it? But to make much more serious and bigger applications, we recommend using more solid technologies, such as TypeScript, React, SCSS, etc.

So, if we want to create a Mini App, we should create a standard web application with any technology stack. The only thing  needs from the developer is the application URL. This URL will be used as a source for the WebView component of the  client, which will load and display the application within .

# Usage
As we mentioned in the previous section, Mini Apps are add-ons for DeJoy Bots. DeJoy Bots are also a well-known technology that provides functionality for a wide range of use cases. You could create a bot to buy a ticket in the cinema, tell jokes, generate random numbers, etc. In other words, the bot can do whatever the developer can think of.

The problem is, the visual part of bots is not as good and functional as it could be. Their current implementation is "console-like" which is more appropriate for developers, not common users. That's where Mini Apps are useful.

Using Mini Apps, developers can create more user-friendly and complex interfaces, which are commonly used by typical users. With this technology, the developer is still able to communicate with the bot behind Mini App, but, additionally, they can provide some more flexible interfaces to interact with.

Mini Apps are used when a standard bot interface is not enough. Create a Mini App when you want to make user life easier when displaying several buttons is not even close to the functionality you want to provide.

# Supported Applications
Currently,  Mini Apps are available on a wide list of  applications:

-  DeJoy for Android: [android]();
-  DeJoy for iOS ios: [ios]();

# Modules and Links

## Functional Features
- **Closing Behavior**: [functional-features/closing-hehavior.md](functional-features/closing-hehavior.md)
- **Swipe Behavior**: [functional-features/swipe-behavior.md](functional-features/swipe-behavior.md)
- **Haptic Feedbacks**: [functional-features/haptic-feedbacks.md](functional-features/haptic-feedbacks.md)

## Apps Communication
- **Flow Definition**: [apps-communication/flow-definition.md](apps-communication/flow-definition.md)
- **Methods**: [apps-communication/methods.md](apps-communication/methods.md)
- **Events**: [apps-communication/events.md](apps-communication/events.md)

## Launch Parameters
- **About**: [launch-parameters/about.md](launch-parameters/about.md)
- **Start Parameter**: [launch-parameters/start_parameter.md](launch-parameters/start_parameter.md)
- **Init Data**: [launch-parameters/init-data.md](launch-parameters/init-data.md)

## Development
- **Debugging**: [development/debugging.md](development/debugging.md)



## UI
- **Back Button**: [ui/back-button.md](ui/back-button.md)
- **Main Button**: [ui/main-button.md](ui/main-button.md)
- **Popup**: [ui/popup.md](ui/popup.md)
- **Settings Button**: [ui/settings-button.md](ui/settings-button.md)
- **Theming**: [ui/theming.md](ui/theming.md)
- **Viewport**: [ui/viewport.md](ui/viewport.md)


## Guides
- **Authorizing User**: [guides/authorizing-user.md](guides/authorizing-user.md)
- **Creating New App**: [guides/creating-new-app.md](guides/creating-new-app.md)
- **Getting App Link**: [guides/getting-app-link.md](guides/getting-app-link.md)
- **Sticky APP**: [guides/sticky-app.md](guides/sticky-app.md)
- **Subpage Sharing and Source Tracking**: [guides/subpage-sharing.md](guides/subpage-sharing.md)