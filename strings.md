> Excerpt From: Apple Education. “Intro to App Development with Swift”. Apple Inc. - Education, 2017. Apple Books. https://itunes.apple.com/ru/book/intro-to-app-development-with-swift/id1118575552?l=en&mt=11

Strings
=====
## Unicode

Unicode is an international standard that can represent almost any character from any language in a standard way.

Strings in Swift are fully Unicode-compliant, so you can create strings that contain the text of different languages:
```swift
let englishGreeting = "Hello, World!"

let chineseGreeting = "你好，世界!"

let spanishGreeting = "¡Hola Mundo!"

let russianGreeting = "Привет мир!"

let japaneseGreeting = "こんにちは世界!"
```

You can also use emoji in names. That can be fun in small doses, but many programmers find it difficult to type, difficult to read, and less expressive than using words for names. *Don't use them, they are just for fun*
```swift
let 🍓🍏🍒🍐🍋🍇 = "Fruit Salad"
```

## Combining Strings

In Swift, you can combine strings by adding them together:
```swift
let username = "Chris"
let likesYourPostMessage = "likes your post"

// Combine strings by using the plus sign
let finishedMessage = username + " " + likesYourPostMessage
```
