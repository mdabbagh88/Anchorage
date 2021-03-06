# Anchorage

[![Version](https://img.shields.io/cocoapods/v/Anchorage.svg?style=flat)](http://cocoadocs.org/docsets/Anchorage)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)

A lightweight collection of intuitive operators and utilities that simplify iOS layout code. Anchorage is built directly on top of the `NSLayoutAnchor` API, so fully supports `UILayoutGuide`.
Each equation or inequality acts on one or more `NSLayoutAnchor`s, and returns active `NSLayoutConstraint`s.

**Anchorage sets the `translatesAutoresizingMaskIntoConstraints` property to `false` on the *left* hand side of the (in)equality, so you should never need to set this property manually.**

# Installation

## CocoaPods

[CocoaPods](http://cocoapods.org/) is a dependency manager for Cocoa projects.
You can install it with the following command:

```sh
$ gem install cocoapods
```

To integrate Anchorage into your Xcode project using CocoaPods, specify it in
your Podfile:

```ruby
platform :ios, '9.0'
use_frameworks!

target '<Your Target Name>' do
  pod 'Anchorage', '~> 2.0'
end
```
Then, run the following command:

```sh
$ pod install
```

## Carthage

[Carthage](https://github.com/Carthage/Carthage) is a decentralized dependency
manager that builds your dependencies and provides you with binary frameworks.

You can install Carthage with [Homebrew](http://brew.sh/) using the following commands:

```sh
$ brew update
$ brew install carthage
```

To integrate Anchorage into your Xcode project using Carthage, specify it in
your Cartfile:

```
github "Raizlabs/Anchorage" ~> 2.0
```

Run `carthage update` to build the framework and drag the built
`Anchorage.framework` into your Xcode project.

# Usage

## Spacing

```swift
// Pin the button to at least 12 pts from the edges of its container view, and center it
button.leftAnchor    >= container.leftAnchor + 12
button.centerXAnchor == container.centerXAnchor
button.centerYAnchor == container.centerYAnchor
```

```swift
// Pin all edges of a view to its container
imageView.edgeAnchors == container.edgeAnchors
```

```swift
// Pin only leading and trailing edges of a view to be within its container
// Note that in this case the >= is interpreted to mean "leading >=, trailing <="
// Additionally, the constant is interpreted to mean "+10 from leading and -10 from trailing"
imageView.horizontalAnchors >= container.horizontalAnchors + 10
```

```swift
// Pin only top and bottom edges of a view to be within 10pt of its container
// Note that in this case the >= is interpreted to mean "top >=, bottom <="
// Additionally, the constant is interpreted to mean "+10 from the top and -10 from the bottom"
imageView.verticalAnchors >= container.verticalAnchors + 10
```

## Sizing

```swift
// Constrain a view's width to be at most 100 pts
view.widthAnchor <= 100

// Constrain two views to be the same size
view1.widthAnchor  == view2.widthAnchor
view1.heightAnchor == view2.heightAnchor

// Constrain view to 4:3 aspect ratio
view.widthAnchor == 4 * view.heightAnchor / 3
```

## Priority

Priority of `NSLayoutConstraints` must be set before they are active.
The `~` is used to specify priority of the constraint resulting from an equation:

```swift
// Align view 20 points from the center of its superview, with low priority
view.centerXAnchor == (view.superview.centerXAnchor + 20) ~ UILayoutPriorityDefaultLow
```

UIKit provides `UILayoutPriorityDefaultLow`, `UILayoutPriorityDefaultHigh`, and `UILayoutPriorityRequired` constants,
but you may specify any `Float` for a priority value.

## Storing Constraints

To store constraints created by Anchorage, simply assign the expression to a variable:

```swift
// A single (active) NSLayoutConstraint
let topConstraint = (imageView.topAnchor == container.topAnchor)

// EdgeConstraints represents a collection of constraints
// You can retrieve the NSLayoutConstraints individually,
// or get an [NSLayoutConstraint] via .all, .horizontal, or .vertical
let edgeConstraints = (button.edgeAnchors == container.edgeAnchors).all
```
