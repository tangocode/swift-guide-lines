## Code Guidelines
### Code Documentation
Documentation comments are distinguished by using /** ... */ for multi-line comments or /// for single-line comments. Inside comment blocks, the conventions you’ve gotten used to when writing Markdown everywhere else apply:
* Paragraphs are separated by blank lines
* Unordered lists can use a variety of bullet characters: -, +, *, •
* Ordered lists use Arabic numerals (1, 2, 3, …) followed by a period 1. or right parenthesis 1):
* Headers can be marked with preceding # signs or by underlining with = or -.
* Even links and images work, with web-based images pulled down and displayed directly in Xcode.
```Swift
/**
    # Lists

    You can apply *italic*, **bold**, or `code` inline styles.

    ## Unordered Lists

    - Lists are great,
    - but perhaps don't nest
    - Sub-list formatting

      - isn't the best.

    ## Ordered Lists

    1. Ordered lists, too
    2. for things that are sorted;
    3. Arabic numerals
    4. are the only kind supported.
*/
```
### Class Header Comments
Each new class or struct has to have a header explaining what the class does, the author and the created date.
```Swift
/**
This class was designed and implemented to help people covert temperatures between the Fahrenheit and Celsius scales.

author: patrickclose92

created: 07/01/2016
*/
```
#### Method Comments
It's important to document each method that adds new functionality to the app, explaining with a short description what the method does, who is the author, the created date, the parameters, throws, and the return.
```Swift
/**
    Repeats a string `times` times.
    
    author: patrickclose
    created: 07/01/2016
    
    - Parameter str:   The string to repeat.
    - Parameter times: The number of times to repeat `str`.

    - Throws: `MyError.InvalidTimes` if the `times` parameter 
        is less than zero.

    - Returns: A new string with `str` repeated `times` times.
*/
func repeatString(str: String, times: Int) throws -> String {
    guard times >= 0 else { throw MyError.InvalidTimes }
    return Repeat(count: 5, repeatedValue: "Hello").joinWithSeparator("")
}
```
### Naming
Use descriptive names with camel case for classes, methods, variables, etc. Class names should be capitalized, while method names and variables should start with a lower case letter.
##### Prefered:
```Swift
private let maximumWidgetCount = 100

class WidgetContainer {
  var widgetButton: UIButton
  let widgetHeightPercentage = 0.85
}
```
##### Not Predered:
```Swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
  var wBut: UIButton
  let wHeightPct = 0.85
}
```
#### Constants
Constants used within type definitions should be declared static within a type. For example:
```Swift
struct PhysicsModel {
    static var speedOfLightInAVacuum = 299_792_458
}

class Spaceship {
    static let topSpeed = PhysicsModel.speedOfLightInAVacuum
    var speed: Double

    func fullSpeedAhead() {
        speed = Spaceship.topSpeed
    }
}
```
Making the constants static allow them to be referred to without needing instances of the type.
Constants at global level should generally be avoided except for singletons.
#### Computed Properties
Use the short version of computed properties if you only need to implement a getter. For example, prefer this:
```Swift
class Example {
    var age: UInt32 {
        return arc4random()
    }
}
```
to this:
```Swift
class Example {
    var age: UInt32 {
        get {
            return arc4random()
        }
    }
}
```




### Enumerations
Use UpperCamelCase for enumeration values:
```Swift
enum Shape {
  case Rectangle
  case Square
  case Triangle
  case Circle
}
```
#### Spaces
* Method braces and other braces (if/else/switch/while etc.) always open on the same line as the statement but close on a new line.
* Tip: You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu). Some of the Xcode template code will have 4-space tabs hard coded, so this is a good way to fix that.
```Swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
```
##### Whitespace
* Tabs, not spaces.
* End files with a newline.
* Make liberal use of vertical whitespace to divide code into logical chunks.
* Don’t leave trailing whitespace.
* Not even leading indentation on blank lines.

### Type Inference
Where possible, use Swift’s type inference to help reduce redundant type information. For example, prefer:
##### Prefered:
```Swift
var currentLocation = Location()
```
##### Not Prefered:
```Swift
var currentLocation: Location = Location()
```
### Self Inference
Let the compiler infer self in all cases where it is able to. Areas where self should be explicitly used includes setting parameters in init, and non-escaping closures. For example:
```Swift
struct Example {
    let name: String

    init(name: String) {
        self.name = name
    }
}
```
### Parameter List Inference
Specifying parameter types inside a closure expression can lead to rather verbose code. Only specify types if needed.

##### Not Prefered:
```Swift
let people = [
    ("Mary", 42),
    ("Susan", 27),
    ("Charlie", 18),
]

let strings = people.map() {
    (name: String, age: Int) -> String in
    return "\(name) is \(age) years old"
}
```
##### Prefered:
```Swift
let strings = people.map() {
    (name, age) in
    return "\(name) is \(age) years old"
}
```
#### Protocol Conformance
When adding protocol conformance to a class, prefer adding a separate class extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

Also, don't forget the `// MARK: - comment` to keep things well-organized!
##### Prefered:
```Swift
class MyViewcontroller: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewcontroller: UIScrollViewDelegate {
  // scroll view delegate methods
}
```
##### Not Prefered:
```Swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

#### Closure Expressions
Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.
##### Prefered:
```Swift
UIView.animateWithDuration(1.0) {
  self.myView.alpha = 0
}

UIView.animateWithDuration(1.0,
  animations: {
    self.myView.alpha = 0
  },
  completion: { finished in
    self.myView.removeFromSuperview()
  }
)
```
##### Not Predered:
```Swift
UIView.animateWithDuration(1.0, animations: {
  self.myView.alpha = 0
})

UIView.animateWithDuration(1.0,
  animations: {
    self.myView.alpha = 0
  }) { f in
    self.myView.removeFromSuperview()
}
```
#### Control Flow
Prefer the `for-in` style of `for` loop over the for-condition-increment style.
##### Prefered
```Swift
for _ in 0..<3 {
  println("Hello three times")
}

for (index, person) in attendeeList.enumerate() {
  println("\(person) is at position #\(index)")
}
```
##### Not Prefered
```Swift
for var i = 0; i < 3; i++ {
  println("Hello three times")
}

for var i = 0; i < attendeeList.count; i++ {
  let person = attendeeList[i]
  println("\(person) is at position #\(i)")
}
```
#### Struct Initializers
Use the native Swift struct initializers rather than the legacy CGGeometry constructors.
##### Prefered:
```Swift
let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)
let centerPoint = CGPoint(x: 96, y: 42)
```
##### Not Prefered:
```Swift
let bounds = CGRectMake(40, 20, 120, 80)
let centerPoint = CGPointMake(96, 42)
```

## Programming Best Practices
### Design patterns
Using design patterns helps to come out with cool solutions and really clean code, this is a good repository with design patterns implementd in swift: https://github.com/ochococo/Design-Patterns-In-Swift#structural

### Optionals
#### Avoid Using Force-Unwrapping of Optionals
If you have an identifier foo of type `FooType?` or `FooType!`, don't force-unwrap it to get to the underlying value (foo!) if possible.

Instead, prefer this:
```Swift
if let foo = foo {
    // Use unwrapped `foo` value in here
} else {
    // If appropriate, handle the case where the optional is nil
}
```
Alternatively, you might want to use Swift's Optional Chaining in some of these cases, such as:
```Swift
// Call the function if `foo` is not nil. If `foo` is nil, ignore we ever tried to make the call
foo?.callSomethingIfFooIsNotNil()
```
*Rationale:* Explicit if let-binding of optionals results in safer code. Force unwrapping is more prone to lead to runtime crashes.

#### Avoid Using Implicitly Unwrapped Optionals
Where possible, use let foo: `FooType?` instead of `let foo: FooType!` if foo may be nil (Note that in general, ? can be used instead of !).

*Rationale:* Explicit optionals result in safer code. Implicitly unwrapped optionals have the potential of crashing at runtime.
#### Prefer structs over classes
Unless you require functionality that can only be provided by a class (like identity or deinitializers), implement a struct instead.

Note that inheritance is (by itself) usually not a good reason to use classes, because polymorphism can be provided by protocols, and implementation reuse can be provided through composition.

For example, this class hierarchy:
```Swift
class Vehicle {
    let numberOfWheels: Int

    init(numberOfWheels: Int) {
        self.numberOfWheels = numberOfWheels
    }

    func maximumTotalTirePressure(pressurePerWheel: Float) -> Float {
        return pressurePerWheel * Float(numberOfWheels)
    }
}

class Bicycle: Vehicle {
    init() {
        super.init(numberOfWheels: 2)
    }
}

class Car: Vehicle {
    init() {
        super.init(numberOfWheels: 4)
    }
}
```
could be refactored into these definitions:
```Swift
protocol Vehicle {
    var numberOfWheels: Int { get }
}

func maximumTotalTirePressure(vehicle: Vehicle, pressurePerWheel: Float) -> Float {
    return pressurePerWheel * Float(vehicle.numberOfWheels)
}

struct Bicycle: Vehicle {
    let numberOfWheels = 2
}

struct Car: Vehicle {
    let numberOfWheels = 4
}
```
*Rationale:* Value types are simpler, easier to reason about, and behave as expected with the let keyword.
#### Error Handling
Swift 2's `do/try/catch` mechanism is fantastic. Use it.
```Swift
do {
    try somethingThatMightThrow()
}
catch {
    fatalError("Something bad happened.")
}
```
to:
```Swift
try! somethingThatMightThrow()
```
##### Avoid try? where possible
`try?` is used to "squelch" errors and is only useful if you truly don't care if the error is generated. In general though, you should catch the error and at least log the failure.

#### Avoid ! where possible
In general prefer `if let, guard let, and assert` to `!`, whether as a type, a property/method chain, `as!`, or (as noted above) `try!`. It’s better to provide a tailored error message or a default value than to crash without explanation. Design with the possibility of failure in mind.

As an author, if you do use `!`, consider leaving a comment indicating what assumption must hold for it to be used safely, and where to look if that assumption is invalidated and the program crashes. Consider whether that assumption could reasonably be invalidated in a way that would leave the now-invalid `!` unchanged.

As a reviewer, treat `!` with skepticism.

#### Early Returns & Guards

When possible, use `guard` statements to handle early returns or other exits (e.g. fatal errors or thrown errors).

Prefer:
```Swift
guard let safeValue = criticalValue else {
    fatalError("criticalValue cannot be nil here")
}
someNecessaryOperation(safeValue)
```
to: 
```Swift
if let safeValue = criticalValue {
    someNecessaryOperation(safeValue)
} else {
    fatalError("criticalValue cannot be nil here")
}
```
of:
```Swift
if criticalValue == nil {
    fatalError("criticalValue cannot be nil here")
}
someNecessaryOperation(criticalValue!)
```
This flattens code otherwise tucked into an `if let` block, and keeps early exits near their relevant condition instead of down in an else block.

Even when you're not capturing a value (`guard let`), this pattern enforces the early exit at compile time. In the second if example, though code is flattened like with guard, accidentally changing from a fatal error or other return to some non-exiting operation will cause a crash (or invalid state depending on the exact case). Removing an early exit from the else block of a guard statement would immediately reveal the mistake.

#### Use functional programming
Swift comes with three functions `map, reduce, filter` to work with arrays that are efficient and avoid verbosity in the code. For instance you can converte from:
```Swift
var source = [1, 3, 5, 7, 9]
var result = [Int]()
for i in source {
    let timesTwo = i * 2
    if timesTwo > 5 && timesTwo < 15 {
        result.append(timesTwo)
    }
}
```
to:
```Swift
let result2 = source.map { $0 * 2 }
                    .filter { $0 > 5 && $0 < 15 }
```
