> Excerpt From: Apple Education. “Intro to App Development with Swift”. Apple Inc. - Education, 2017. Apple Books. https://itunes.apple.com/ru/book/intro-to-app-development-with-swift/id1118575552?l=en&mt=11

# Lazy Stored Properties
A *lazy stored property* is a property whose initial value is not calculated until the first time it is used. You indicare a lazy stored property by writing the `lazy` modifier before its declaration.

> You must always declare a lazy property as a variable (with the `var` keyword), because its initial value might not be retrieved until after instance initialization completes. Constant must always have a value *before* initialization complates, and therefore cannot be declared as lazy.

Lazy properties are useful when the initial value for a propery is dependent on outside factors whose values are not known until after an instance's initialization is complete. Lazy properties are also useful when the initial value for a property requires complex or computationally expensive setup that should not be performed unless or until it is needed.

The example below uses a lazy stored property to avoid unnecessary initialization of a complex class. This example defines two classes called `DataImporter` and `DataManager`, neither of which is shown in full:

```swift
class DataImporter {
  /* DataImporter is a class to import data from an external file.
  The class is assumed to take non-trivial amount of time to initialize.
 */
 
  var filename = "data.txt"
 // the DataImporter class would provide data importing functionality here
}

class DataManager {
  lazy var importer = DataImporter()
  var data = [String]()
  // the DataManager class would provide data management functionality here
}

let manager = DataManager()
manager.data.append("Some data")
manager.data.append("Some more data")

// the DataImporter instance for the importer property has not yet been created
```

The `DataManager` class has a stored property called `data`, which is initialized with a new, empty array of `String` values. Although the rest of its functionality is now shown, the purpose of this `DataManager` class is to manage and provide access to this array of `String` data.

Part of the functionality of the `DataManager` class is the ability to import data from a file. This functionality is provided by the `DataImporter` class, which is assumed to take non-trivial amount of time to initialize. This might be because a `DataImporter` instance needs to open a file and read its content into memory when the `DataImporter` instance is initialized.

It is possible for a `DataManager` instance to manage its data without ever importing data from a file, so there is no need to create a new `DataImporter` instance when the `DataManager` itself is created. Instead, it makes more sense to create the `DataImporter` instance if and when it is first used.

Because it is marked with the `lazy` modifier, the `DataImporter` instance for the `importer` property is only created when the `importer` property is first accessed, such as when its `filename` property is queried:

```swift
print(manager.importer.filename)

// the DataImporter instance for the importer property has now been created
// prints "data.txt"
```

> If a property marked with the `lazy` modifier is accessed by multiple threads simultaneously and the property has not yet been initialized, there is no guarantee that the property will be initialized only once.
