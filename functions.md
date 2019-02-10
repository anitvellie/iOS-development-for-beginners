> Excerpt From: Apple Inc. “The Swift Programming Language”. 
> Apple Books. https://itunes.apple.com/ru/book/the-swift-programming-language-swift-4-2/

Swift Basics
======
Functions
------
*Functions* are self-contained chuncks of code that perform a specific task. Swift's unified function syntax is flexible enough to express anything from a simple C-style function with no parameter names to a complex Objective-C-style method with local and external names for each parameter. Parameters can provide defult values to simplify function calls ans can be passed as in-out parameters, which modify a passed variable once the function has completed its execution.

Every function in Swift has a type, consisting of the function's parameter types and return type. You can use this type like any other type in Swift, which makes it easy to pass functions as parameters to other functions, and to return functions from functions. Functions can also be written within other functions to encapsulate useful functionality within a nested function scope.

### Defining and Calling Functions
When you define a function, you can optionally define one or more named, typed values that the function takes as input (known as *parameters*), and/or a type of value that the function will pass back as output when it is done (known as *return type*).

The function in the example below is called `sayHello`, because that's what it does – it take a person's name as input and returns a greeting for that person. To accomplish this, you define one input parameter – a `String` value called `personName` – and a return type of `String`, which will contain a greeting for that person:

```swift
func sayHello(personName: String) -> String {
  let greeting = "Hello, " + personName + "!"
  return greeting
}
```

All of this information is rolled up into the function's *definition*, which is prefixed with the `func` keyword. You indicate the function's return type with the *return arrow* `->`. which is followed by the name of the type to return.

The definition describes what the function does, what it expects to receive, and what it returns when it is done. The definition makes it easy for the function to be called unambiguously from elsewhere in your code:

```swift
println(sayHello("Anna"))
// prints "Hello, Anna!"
```

### Function Parameters and Return Values
Function parameters and return values are extremely flexible in Swift. You can define anything from a simple utility function with a single unnamed parameter to a complex function with expressive parameter names and different parameter options.

#### Multiple Input Parameters
Function can have multiple input parameters, which are written within the function's parantheses, separated by commas. 

This function takes a start and an end index for a half-open range, and works out how many elements the range contains:

```swift
func halfOpenRangeLength(start: Int, end: Int) -> Int {
  return end - start
}
println(halfOpenRangeLength(1, 10))
// prints "9"
```

#### Functions Without Parameters
Functions are not required to define input parameters. Here's a function with no input parameters, which always returns the same `String` message whenever it is called:

```swift
func sayHelloWorld() -> String {
  return "hello, world"
}

println(sayHelloWorld())
// prints "hello, world"
```

#### Functions Without Return Value
Functions are not required to define a return type. Here's a version of the `sayHello` function, called `sayGoodbye`, which prints its own `String` value rather than returning it:

```swift
func sayGoodbye(personName: String) {
  prints("Goodbye, \(personName)!")
}

sayGoodbye("Dave")
// prints "Goodbye, Dave!"
```

Because it does not need to return a value, the function's definition does not include the return arrow (`->`) or a return type.

> Strictly speaking, the `sayGoodbye` function *does* still return a value, even though no return value is defined. Functions without a defined return type return a special value of type `Void`. This is simply an empty tuple, in effect a tuple with zero elements, which can be written as `()`.

The return values of a function can be ignored when it is called:

```swift
func printAndCount(stringToPrint: String) -> Int {
  println(stringToPrint)
  return count(stringToPrint)
}
func printWithoutCounting(stringToPrint: String) {
  printAndCount(stringToPrint)
}

printAndCount("hello, world")
// prints "hello world" and returns a value of 12

printWithoutCounting("hello, world")
// prints "hello world" but does not return anything
```

> Return values can be ignored, but a function that says it will return a value must always do so. A function with a defined return type cannot allow control to fall out of the botton of the function without returning a value, and attempting to do so will result in a complile-time error.

#### Functions with Multiple Return Values
You can use a tuple type as the return type for a function to return multiple values as part of one compound return value.

The example below defines a function called `minMax`, which finds the smallest and largest numbers in an array of `Int` values:

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
  var currentMin = array[0]
  var currentMax = array[0]
  for value in array[1..<array.count] {
    if value < currentMin {
      currentMin = value
    } else if value > currentMax {
      currentMax = value
    }
  }
  return (currentMin, currentMax)
}
```

In this function, the overall minimum and maximum values are returned as a tuple of two `Int` values. 
Because the tuple's member values are named as part of the function's return type, they can be accessed with dot syntax to retrieve the minimum and maximum found values:

```swift
let bounds = minMax([8, -6, 2, 109, 3, 71])
println("min is \(bounds.min) and max is \(bounds.max)")
// prints "min is -6 and max is 109"
```

Note that the tuple's members do not need to be named at the point that the tuple is returned from the function, because their names are already specified as part of the function's return type.

#### Optional Tuple Return Types
If the tuple type to be returned from a function has the potential to have "no value" for the entire tuple, you can use an *optional* tuple return type to reflect the fact that the entire tuple can be `nil`. You write an optional tuple return type be placing a question mark after the tuple type's closing paranthesis, such as `(Int, Int)?` or `(String, Int, Bool)?`.

> An optional tuple type such as `(Int, Int)?` is different from a tuple that contains optional types such as `(Int,?, Int?)`. With an optional tuple type, the entire tuple is optional, not just each individual value within the tuple.

### Function Parameter Names
All of the above functions define *parameter names* for their parameters.

```swift
func someFunction(parameterName: Int) {
  // function body goes here
  // you can use parameterName to refer to the argument value for that parameter
}
```

However, these parameter names are only used within the body of the function itself, and cannot be used when calling the function. These kinds of parameter names are known as *local parameter names*, because they are only available for use within the function's body.

#### External Parameter Names
Sometimes it's useful to name each parameter when you *call* a function, to indicate the purpose of each argument you pass to the function. 

If you want users of your function to provide parameter names when they call your function, define an *externam parameter name* for each parameter, in addition to the local parameter name. You write an external parameter name before the local parameter name it supports, separated by a space:

```swift
func someFunction(externalParameterName localParameterName: Int) {
  // function body goes here, and you use localParameterName 
  // to refer to the argument value for that parameter
}
```

> If you provide an external parameter name for a parameter, that external name must *always* be used when you call the function.

As an example, consider the following function, which joins two strings by inserting a third "joiner" string between them:

```swift
func join(s1: String, s2: String, joiner: String) -> String {
  return s1 + joiner + s2
}
```

When you call this function, the purpose of the three strings that you pass to the function is unclear:

```swift
join("hello", "world", ", " )
// returns "hello, world"
```

To make the purpose of these `String` values clearer, define external parameter names for each `join` function parameter:

```swift
func join(string s1: String, toString s2: String, withJoiner joiner: String) -> String {
  return s1 + joiner + s2
}
```

You can now use these external parameter names to call the function unambiguously:

```swift
join(string: "hello", toString: "world", withJoiner: ", ")
// returns "hello, world"
```

The use of external parameter names enables this second version of the `join` function to be called in an expressive, sentence-like manner by users of the function, while still providing a function body that is readable and clear in intent.

> Consider using external parameter names whenever the purpose of a function's arguments would be unclear to someone reading your code for the first time. You do not need to specify external parameter names of the purpose of each parameter is unambiguous when the function is called.

#### Shorthand External Parameter Names
If you want to provide an external parameter name for a function parameter, and the local parameter name is already an appropriate name to use, you do not need to write the same name twice for that parameter. Instead, write the name once, and prefix the name with a hash symbol (`#`). This tells Swift to use that name as both the local parameter name and the external parameter name.

This example defines a function called `containsCharacter`, which defines external parameter names for both of its parameters by placing a hash symbol before their local parameter names:

```swift
func containsCharacter(#string: String, #characterToFind: Character) -> Bool {
  for character in string {
    if character == characterToFind { 
      return true
    }
  }
  return false
}
```
