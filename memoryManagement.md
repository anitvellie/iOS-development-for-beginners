# Memory Management in Swift

> Excerpt From: Apple Education. “Intro to App Development with Swift”. Apple Inc. - Education, 2017. Apple Books. https://itunes.apple.com/ru/book/intro-to-app-development-with-swift/id1118575552?l=en&mt=11

Swift uses ARC (*Automatic Reference Counting*) to track and manage your app's memory usage.

> Reference counting applies only to classes. Structs and enums are value types, not reference types, they are not stored and passed by reference. 

Every time you create an instance of a class, ARC allocates a chunck of memory to store information about that instance. This memory hold the information about the type of the instance, together with values of any ptored properties associated with that instance. 
When the instance is no longer needed, ARC frees up the memory. 
To make sure that instances don't dissapear while they are still in use, ARC tracks how many properties, constants, and variable are currently referencing to each class instance. ARC will not delocate an instance as long as there are at least one active reference to that instance.

## ARC in Action
Here's an example of how ARC works. Let's define a simple class Person:
```swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
        print("\(name) is being initialised")
    }
    deinit {
        print("\(name) is being deinitiased")
    }
}
```
When the instance of the class `Person` is created, it prints the message to the console with the name of the person. When an instance off the class is deinitialised, it prints the similar message to the console.

Let's create three instances of the class `Person`, but make it optional:
```swift 
var reference1: Person?
var reference2: Person?
var reference3: Person?
```
They are automatically initialised with the value `nil` and are not referencing to `Person` yet.
Now let's create an instance of class `Person` an assign it to the first variable:
```swift
reference1 = Person(name: "Mike")
```

You should see the message in the console saying that `Mike is being initialised`. There's now a strong reference from variable `reference1` to the new `Person` instance. When you assign it to more variables, more strong references are created.
```swift
reference2 = reference1
reference3 = reference1
```
There's now *three* strong references to the instance of the class `Person`.
Try to break two of these references by assigning variables to `nil`:
```swift
reference1 = nil
reference2 = nil
```
Notice how the instance `Mike` is still not deinitialised, because there's the third strong reference.
To break that, assign the last variable to `nil`:
```swift
reference3 = nil
```

## Strong Reference Cycles Between Class Instances

In the examples above we tracked the references to the instance and delocated memory when we had zero references left. But it's possible to write code in which an instance of a class *never* gets to a point when it has zero strong references.
This can happen if two instances hold strong references to each other. This is known as *strong reference cycle*.

Here's an example of how to create a strong reference cycle:
Let's add a new `apartement` field to our `Person` class:
```swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
        print("\(name) is being initialised")
    }
    
    var apartment: Apartment?
    
    deinit {
        print("\(name) is being deinitiased")
    }
}
```
And a new `Apartement` class:
```swift
class Apartment {
    let unit: String
    init(unit: String) {
        self.unit = unit
        print("\(unit) is being initialised")
    }
    
    var tenant: Person?
    deinit {
        print("\(unit) is being deinitialised")
    }
}
```
Now let's create two variables and assign them to `Person` and `Apartement` instances:
```swift
var john: Person?
var unit4A: Apartment?

john = Person(name: "John")
unit4A = Apartment(unit: "4A")
```
The `john` variable now has a strong reference to `Person` instance, and the `unit4A` has a strong reference to `Apartement` class.
Now we can link these variables together so the person has an apartement and the apartement has a tenant:

```swift
john!.apartment = unit4A
unit4A!.tenant = john
```
Now these variables do not only have strong references to their instances but also to each other: `john` has a strong reference to instance of `Person` and also to `unit4A`; `unit4A` has a strong reference to instance of `Apartement` and also to `john`.

Let's try to assign these variables to `nil` to deinitialise them:
```swift
john = nil
unit4A = nil
```
Notice how we do not get the message of deinitialisation in the console. That's because there's no deinitialisation happening here! Our variables do not longer reference to instances of their classes, but they are still referencing to each other: `john` has a strong reference to `unit4A`, and `unit4A` has a strong reference to `john`. 
Unfortunately, there's almost nothing we can do in that situation. These two variables will live in our memory forever because there's at least one reference and there's no way to break that now.

## Resolving Strong Reference Cycle
Swift provides two ways of resolving strong reference cycles: *weak* references and *unowned* references.
Weak and unowned references enable one instance in the reference cycle to reference to another one *without* keeping a strong hold on it. 
Use weak reference when the other instance has a shorter lifetime 
