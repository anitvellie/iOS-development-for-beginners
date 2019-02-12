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
## String Interpolation

In Swift, you can define a string with placeholders that will be replaced with values. It works a lot like the fill-in-the-blanks example: “Hello _______, welcome to _______!”. 
You don't use blank spaces as placeholders in Swift. You use *\(name)*. The value of name replaces the placeholder.

```swift
let firstName = "Tim"
let city = "Cupertino"

let welcomeString = "Hello \(firstName), welcome to \(city)"
```

String interpolation is a powerful way to build strings. In addition to substituting string values, you can substitute in other values too, like numbers or even calculations.
```swift
let goalieName = "Alison"
let firstHalfSaves = 3
let secondHalfSaves = 6
let overtimeSaves = 2
let goalieReportString = "At the game yesterday, \(goalieName) had \(firstHalfSaves) saves in the first half, \(secondHalfSaves) in the second half and \(overtimeSaves) saves in overtime, for a total of \(firstHalfSaves + secondHalfSaves + overtimeSaves) saves."
```

To include a quotation mark in a string, type a backslash before the quotation mark:
```swift
let stringWithQuotationMarks = "He said, \"Hi there!\" as he passed by."
```
The backslash tells Swift to treat what comes next as special. Since the quotation mark character follows the backslash, Swift treats it differently. It includes the quotation mark in the string, rather than ending the definition of the string.

Because the backslash character is how you “escape” from the normal behavior of a string, it’s called an *escape character*.

The pattern of an escape character followed by something that’s treated specially is called an escape sequence.
```swift
// The backslash followed by a quotation mark is an escape sequence.
let favoriteQuotation = "Hamlet said, \"To be, or not to be?\""
```
