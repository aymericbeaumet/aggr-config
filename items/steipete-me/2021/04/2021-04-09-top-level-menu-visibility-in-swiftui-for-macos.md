---
title: Top-Level Menu Visibility in SwiftUI for macOS
link: https://steipete.me/posts/2021/top-level-menu-visibility-in-swiftui/
source: steipete-me
published: 2021-04-09T16:00:00Z
updated: 2021-04-09T16:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Working around SwiftUI's CommandsBuilder limitations to conditionally show top-level menus on macOS using direct AppKit integration.
content: extracted
html: 2021-04-09-top-level-menu-visibility-in-swiftui-for-macos.html
preview:
  file: 2021-04-09-top-level-menu-visibility-in-swiftui-for-macos.preview-382702ddc0d1.webp
  width: 256
  height: 134
  color: '#e8e6e6'
images:
- source: https://steipete.me/posts/2021/top-level-menu-visibility-in-swiftui/index.png
  original:
    file: 2021-04-09-top-level-menu-visibility-in-swiftui-for-macos.image-406d467219ab.png
    width: 1200
    height: 630
  variants:
  - file: 2021-04-09-top-level-menu-visibility-in-swiftui-for-macos.image-4a0045524bf7.webp
    width: 320
    height: 168
  - file: 2021-04-09-top-level-menu-visibility-in-swiftui-for-macos.image-a0ba1ee1e428.webp
    width: 640
    height: 336
  - file: 2021-04-09-top-level-menu-visibility-in-swiftui-for-macos.image-03be6e347c8f.webp
    width: 960
    height: 504
  - file: 2021-04-09-top-level-menu-visibility-in-swiftui-for-macos.image-e365ae3fabc7.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2021/top-level-menu-visibility-swiftui/flow-statement.png
  original:
    file: 2021-04-09-top-level-menu-visibility-in-swiftui-for-macos.image-c614282c0264.png
    width: 1780
    height: 298
  color: '#fefdfd'
---

![](https://steipete.me/assets/img/2021/top-level-menu-visibility-swiftui/flow-statement.png)

Pretty much all Mac apps have a semi-hidden Debug menu that can be triggered via a user defaults entry or via settings. Naturally I wanted to add the same in my latest project. I’m building a new “universal” app (meaning iOS *and* macOS), supporting only the latest OSes, so I can using the new SwiftUI app lifecycle.

SwiftUI is really a lot of fun to work with. Sure, [there are bugs, warts](https://steipete.me/posts/state-of-swiftui/) and parts that simply aren’t finished yet, especially on the Mac, but overall what Apple built here is really great, and it’s so much faster to build apps with it. SwiftUI makes the hard things simple, and sometimes it makes the simple things hard.

Let’s look at a typical menu definition in the new Big Sur/iOS 14 SwiftUI App Lifecycle. The syntax is straightforward and fits right into the concepts of SwiftUI. Bingings work as well and menus change on-demand as state changes.

```swift
@main
struct SampleApp: App {
    var body: some Scene {
        WindowGroup {
            MainAppView()
        }
        .commands {
            CommandGroup(replacing: CommandGroupPlacement.newItem) {
                Button("Import Archive") {
                    activeSheet = .importer
                }
                .keyboardShortcut(KeyEquivalent("i"), modifiers: .command)
            }
    }
}
```

There’s a superb guide over at [TrozWare about SwifUI Mac Menus](https://troz.net/post/2021/swiftui_mac_menus/) that explains everything in detail - including a way how to move the menu logic into a separate file. Highly recommended. Let’s move on to the interesting bits.

Within `CommandMenu` it’s easy to use `if`/`else` to conditionally show menu entries. SwiftUI uses `@ViewBuilder` as resultbuilder and conditionals are correctly implemented.

```swift
CommandMenu("Animals") {
    if user.likesCats {
        Button("Show Cat Picture") { }
    } else {
        Button("Show Dog Picture") { }
    }
```

However if we try the same at the top level, we get an error: `"Closure containing control flow statement cannot be used with result builder 'CommandsBuilder'"`gs. The SwiftUI-team didn’t implement any branching logic into the `@CommandsBuilder`.

After [a discussion on Twitter](https://twitter.com/steipete/status/1380518850073092096?s=21), there really doesn’t seem a SwiftUI-way to trigger the visibility of top-level menus. @LeoNatan suggested to [drop back into AppKit](https://twitter.com/leonatan/status/1380545179157925888?s=21), and that’s what I ended up doing:

```swift
static func triggerDebugMenuVisibilityHack() {
    if let mainMenu = NSApp.mainMenu {
        DispatchQueue.main.async {
            if !Features.shared.isDebugModeEnabled,
               let debugMenu = mainMenu.items.first(where: { $0.title == "Debug" }) {
                mainMenu.removeItem(debugMenu)
            }
        }
    }
}
```

Make sure to trigger this both on app start and whenever the debug value changes:

```swift
.onAppear {
    DebugMenuCommands.triggerDebugMenuVisibilityHack()
}
.onChange(of: features.isDebugModeEnabled) { _ in
    DebugMenuCommands.triggerDebugMenuVisibilityHack()
}
```

And that’s it. Toggling the menu works just as expected. In our update method we have to skip a runloop so the SwiftUI glue has time to set up the menu, however this gets called so early in the app startup lifecycle that it’s not visible. So while not the most elegant solution, this works perfectly fine.

## Conclusion

This is a good reminder that even when writing a “Pure SwiftUI” application, the underlying frameworks are there and can help you whenever you run into a limitation of SwiftUI. Since this feels like an omission, I’ve opened a radar (FB9074334) for the SwiftUI team.
