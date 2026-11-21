# Swift tests

## The standard

- **Framework:** Swift Testing (`import Testing`, `@Test("behavior")`, `#expect`, `#require`),
  shipped with Swift 6 on macOS and Linux. XCTest is the older one; use it only where a
  project already does, or for UI tests (XCUITest).
- **Runner:** SwiftPM. `swift test --parallel` builds incrementally and runs tests side by
  side. Parametrize with `@Test(arguments:)` instead of copying a test per input.
- **Never** a `main.swift` per check compiled by a shell loop of `swiftc` calls: it has no
  build cache, compiles shared files once per check, and every new check copies it.

## An app's logic in a package

An iOS app's Xcode target compiles everything as one module. Put the logic that needs only
Foundation in a SwiftPM library target, and test that:

```swift
// ios/Package.swift
// swift-tools-version: 6.0
import PackageDescription
let package = Package(
  name: "AppCore",
  targets: [
    .target(name: "AppCore", path: "App", sources: ["Models/Wire.swift", "Shared/Fmt.swift"]),
    .testTarget(name: "AppCoreTests", dependencies: ["AppCore"], path: "Tests"),
  ]
)
```

Tests use `@testable import AppCore`. A fixture file is found from `#filePath`, not passed
as an argument.

## Linux or Mac

- Foundation, `Testing` and the swift.org toolchain run on Linux, so a package of Foundation
  logic runs on the cheap Linux pool. A few Foundation APIs are Darwin only (`URLRequest`
  needs `import FoundationNetworking` on Linux; some `AttributedString` initializers are
  missing): the Linux build finds them.
- SwiftUI, Charts, UIKit, CryptoKit (swift-crypto is the portable twin) and CoreGraphics
  views need a Mac. Keep them out of the package, and keep the logic they draw in
  Foundation files so it stays testable on Linux.
- A view's rendered output is checked by snapshot testing
  (`pointfreeco/swift-snapshot-testing`) on a Mac, not by reading a screenshot with OCR.

## Static rules

A rule about source text ("every pushed page hides its bar on scroll") is a SwiftLint
custom rule with a message that names the fix, not `grep` in the test suite.
