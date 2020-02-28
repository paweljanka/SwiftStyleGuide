# **Naming**

### **General**

- Use descriptive names.
- Do not use abbreviation for names.
- Use camelCase for:
	- variables
	- constants
	- methods
	- parameters
	- enum cases
- Use PascalCase for:
	- classes
	- structs
	- protocols
	- enums
- Basically, follow official swift [api guidelines](https://swift.org/documentation/api-design-guidelines/#strive-for-fluent-usage)

*Preferred:*

```swift
let answerToTheQuestionOfLife = 42

enum MyEnum {
	case firstCase
	case secondCase
}

protocol MyDelegate {
	func someFunc(param: Int)
}

class SomeClass: BaseClass, MyDelegate {

	var someVariable = 1
	
	func someFunc(param: Int) {
		println(param)
	}
}
```

*Preffered*

```swift
func translate(by x: Int, y: Int)
```

*NOT preffered*

```swift
func translateBy(x: Int, y: Int)
func translateByX(x: Int, y: Int)
```

Don't use class prefixes - we have namespaces for that.

### **Delegates**

When creating custom delegate methods, an unnamed first parameter should be the delegate source. (UIKit contains numerous examples of this.)

*Preffered*
```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

*NOT preffered*
```swift
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

### **Generics**

Generic type parameters should be descriptive, upper camel case names. When a type name doesn't have a meaningful relationship or role, use a traditional single uppercase letter such as T, U, or V.

*Preffered*
```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

*NOT preffered*
```swift
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

### **Type infered context**

Use compiler inferred context to write shorter, clear code.

*Preffered*
```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

*NOT preffered*
```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```




# **Spaces**

### **Spaces in type declaration**

You should add space after type definition colon and use no space before

*Preffered*
```swift
var number: Int
```

*NOT preffered*
```swift
var number : Int
var string:String
```

### **Other spaces** ###

User space on right side of colon `:` in when defining dictionary

*Preffered*
```swift
var dictionary = ["key": "value"]
```

*NOT preffered*
```swift
var dictionary = ["key":"value"]
var otherDictionary = ["key" : 10]
```




# **Indentation**

- Use 4 spaces indentation.
- Don't use tabs.

### **Indentation in `switch`**
Identate in switch statements

*Preffered*
```swift
switch optional {
    case .none:
        // do stuff with none value
    case .value(let value):
        // do stuff with value        
}
```

*NOT preffered*
```swift
switch optional {
case .none:
    // do stuff with none value
case .value(let value):
   // do stuff with value        
}
```




# **Conditional statements** ##

- DO NOT add parenthesis for all `if` and `switch` statements
- Always use brackets for if wrap `if`'s body.
- Write opening bracket in the same line as condition and closing bracket in the new line.
- If condition is very short/simple, use `?:` operator.
- `else` write in the same line as closing bracket of `if` part and separate it  from brackets by single space.
- If `guard` or `if let` statement are short you can write it in one line.

*Preffered*

```swift
if condition {
	...
}

if condition {
	...
} else {
	...
}
```

*NOT preffered*

```swift
if (condition) {
	...
}

if (condition)
{
	...
}

if (condition) {
	return 0 }
	
if (condition) return 1
```




# **Properties**

- Prefer to use constants over variables.
- Don't write a type of property if it could be inferred by compiler.
- If computed property is read-only, omit the get clause.
- If you have to write type of property, write colon exactly after name, then write one space and then write a type of property.
- If your computed property have getter and setter, always write getter before setter.

*Preffered*

```swift
var property1: Int
var property2: Int {
	return 44
}
var property3: Int {
	get {
		return property1
	}
	set(newValue) {
		property1 = newValue
	}
}
```

*NOT preffered*

```swift
var property1 : Int
var property2 :Int
var property3:Int
var property4: Int {
	get {
		return 44
	}
}
var property5: Int {
	set(newValue) {
		property1 = newValue
	}
	get {
		return property1
	}
}
```




# **Structs and classes**

### **Struct vs class usage**

- Do not prefer `struct` over `class` by default. Think about `struct` as value type, just like `Int` or `NSDate`. 

- It ***may*** be good idea to use `struct` as model - passing model by value can be handy, you don't have to worry about some distant part of app changes your data, but on the other hand you have to think about update mechanism when data change.

- But if you think about some service, or repository - struct doesn't fit in this model. Sometimes you have to pass reference of for example existing repository, and it'll be nice to have the same context inside.

- Remember that using structs in Cocoa Touch may be limited for example in `NSNotificationCenter` where you can only pass object in user info dictionary.

### **Compiler optimisations with final** ###

Always use `final` keyword for classes (99% cases) or for functions that are not intended for derivation. It helps compiler with making static optimisation.

### **Struct/class structure**

Use following order for stored and computed properties:

    - public static let
    - public static var
    - public static computed
    - public let
    - public var
    - public computed
    - private static let
    - private static var
    - private static computed
    - private let
    - private var
    - private computed

- **[Test till 28.02]** How to group properties with new lines ?
- Write method as close as possible to its calls. For example if you have public method, which invokes 3 private methods, write these private methods just under the public method, not on the end of file.
- Add one blank line at the begin of class.
- Add one blank line between properties and methods.
- Add one blank line between methods.
- No new line at the end of the class.
- No new line if your struct is just pure data model

*Preffered*

```swift
class MyClass {
	
	let property1 = 1
	let property2 = 2

	var property3 = "Some string"

	var computedProperty {
		get {
			return property2.uppercaseString
		}
		set(newValue) {
			property2 = newValue.lowercaseString
		}
	}

	private property4 = "Private property"
	private property5 = "Private property"
	private property6 = "Private property"
	
	func publicMethod() {
		foo1()
		foo2()
	}
	
	private func foo1() {
		...
	}

	private func foo2() {
		...
	}

	func secondPublicMethod() {
		bar()
	}
	
	private func bar() {
		...
	}
	
}
```

### **Type inference and self**

- Use type inference wherever you can - don't write type of property if it could be inferred by compiler.
- Use self only if it is necessary: when required to differentiate between property names and arguments and when referencing to class properties/methods from closure.

*Preffered*

```swift
class SomeClass {
	
	var x = 5
	var closure = {
		pritln(self.x)
	}	

	init(x: Int) {
		self.x = x
	}
}
```

*NOT preffered*

```swift
class SomeClass {
	
	var x = 5
	
	init(y: Int) {
		self.x = y
	}
	
	func someFunc() -> Int {
		return self.x
	}

}
```

### **Computed properties**

If a computed property is read-only omit the get clause

*Preffered*

```swift
var area: Double {
    return side * side
}
```

*NOT preffered*

```swift
var area: Double {
    get {
        return side * side
    }
}
```

### **Function Declarations**

Don't break function signature lines




# **Optionals**

### **Force unwrap `!`**
- Do not use force unwrap by default
- Use it only when you have very good reason for it, for example

```
class MyViewController: UIViewController {
    var propertyToInject: Type!

    init(property: Type) {
        propertyToInject = property
    }   
}
```
it's because in `UIViewController` we need to override init with coder, in which we can't inject our property, force unwrap is currently the only option.

### **Other**

- Use syntactic sugar for type declaration, such as `[Int]` instead of `Array<Int>`.
- Use syntactic sugar for dealing with optionals wherever you can:
	- multiple `if let`
	- `??` operator
	- optional chaining
	- and many others
- Don't use semicolons - there's so few places when you really need this.




# **Protocols**

- Write all typealiases at the beginning of protocol body.
- Then write all properties declarations.
- Then write all method declarations.
- Don't add blank lines at the beginning or end of protocol body, but you can add blank line in the middle for separating significant parts of protocol definition, such as separate properties from methods.




# **Closures**

- Use trailing closure syntax only if there is only one closure.
- Remove not need information about closure type, such as `-> ()`.
- If closure has only one line (especially when it is a closure for some functions like `map` or `filter`), you can omit types and write it in one line.
- Use wildcard "`_`" for closure arguments you don't use.
- Do not use parentheses for arguments in closure
- Try to capture only those properties which are needed, not just `self`.
- If you capture `self`, remove other properties from capture list.

*Preffered*

```swift
methodWithErrorBlock(someParam) { error in
	...
}
methodWithTwoClosures(successBlock: {
	...
}, errorBlock: { error in
	
})
someIntArray.filter { $0 % 2 }
```

When using chained closures decision on line breaks is left to discretion of the author

```swift
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)

let value = numbers
    .map {$0 * 2}
    .filter {$0 > 50}
    .map {$0 + 10}
```



# **Extending object lifetime**

Extend object lifetime using the `[weak self]` and `guard let self = self else { return }` idiom. `[weak self]` is preferred to `[unowned self]` where it is not immediately obvious that `self` outlives the closure. Explicitly extending lifetime is preferred to optional unwrapping.

*Preffered*
```swift
resource.request().onComplete { [weak self] response in
    guard let self = self else {
        return
    }
    let model = self.updateModel(response)
    self.updateUI(model)
}
```

*NOT preffered*

```swift
// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
    let model = self.updateModel(response)
    self.updateUI(model)
}
```

*NOT preffered*


```swift
// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
    let model = self?.updateModel(response)
    self?.updateUI(model)
}
```




# **Comments**

Should be use sparingly and only to explain why a piece of code does something.


# **Code marks**

- Avoid using `MARK` unless it's necessary for better code separation.
- Use `TODO:` mark to mark places, when you need to fix or complete in near future.

*Preffered*

```swift
// MARK: UIViewController
```

*NOT preffered*

```swift
//MARK: UIViewController
//MARK:UIViewController
```




# **Misc**

### **Lines length**
- Lines need to be less than 120 characters wide,
- If it's longer - we break the line,
- **Exception** - in some cases (e.g. inits) you can leave a function definition line without breaking it.

```swift
func veryLongFunctionNameWithArguments(firstArgument: Int,
                                        second secondArgument: Int,
                                        third thirdArgument: Int,
                                        fourth fourthArgument: Int) {
    callingVeryLongFunctionWithLotOfParameters(
        firstArgument: firstArgument, 
        secondArgument: secondArgument, 
        thirdArgument: thirdArgument
    )	
}
```


# **Singletons**
- Try to avoid using singletons at all.
- Hide it behind protocol and not use directly if you have to.
- DO NOT use without dependency injection.


# **Localization**

- Use with dot syntax.
- Avoid reusing keys.
