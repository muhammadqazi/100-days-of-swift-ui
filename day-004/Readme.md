# Day 004


##### How to use type annotations

Swift is able to figure out what type of data a constant or variable holds based on what we assign to it. However, sometimes we don’t want to assign a value immediately, or sometimes we want to override Swift’s choice of type, and that’s where type annotations come in.

So far we’ve been making constants and variables like this



```
let surname = "Lasso"
var score = 0
```

This uses type inference: Swift infers that surname is a string because we’re assigning text to it, and then infers that score is an integer because we’re assigning a whole number to it.

Type annotations let us be explicit about what data types we want, and look like this

```
let surname: String = "Lasso"
var score: Int = 0
```

Array holds lots of different values, all in the order you add them. This must be specialized, such as [String]:

```
var albums: [String] = ["Red", "Fearless"]
```

Dictionary holds lots of different values, where you get to decide how data should be accessed. This must be specialized, such as [String: Int]:

```
var user: [String: String] = ["id": "@twostraws"]
```

Set holds lots of different values, but stores them in an order that’s optimized for checking what it contains. This must be specialized, such as Set<String>:

```
var books: Set<String> = Set(["The Bluest Eye", "Foundation", "Girl, Woman, Other"])
```

As well as all those, there are enums. Enums are a little different from the others because they let us create new types of our own, such as an enum containing days of the week, an enum containing which UI theme the user wants, or even an enum containing which screen is currently showing in our app.

Values of an enum have the same type as the enum itself, so we could write something like this:


```
enum UIStyle {
    case light, dark, system
}

var style = UIStyle.light
```

This is what allows Swift to remove the enum name for future assignments, so we can write style = .dark – it knows any new value for style must be some kind UIStyle

Now, there’s a very good chance you’ll be asking when you should use type annotations, so it might be helpful for you to know that I prefer to use type inference as much as possible, meaning that I assign a value to a constant or variable and Swift chooses the correct type automatically. Sometimes this means using something like var score = 0.0 so that I get a Double.

The most common exception to this is with constants I don’t have a value for yet. You see, Swift is really clever: you can create a constant that doesn’t have a value just yet, later on provide that value, and Swift will ensure we don’t accidentally use it until a value is present. It will also ensure that you only ever set the value once, so that it remains constant.

For example:

```
let username: String

username = "@twostraws"

print(username)
```

That code is legal: we’re saying username will contain a string at some point, and we provide a value before using it. If the assignment line – username = "@twostraws" – was missing, then Swift would refuse to build our code because username wouldn’t have a value, and similarly if we tried to set a value to username a second time Swift would also complain.

This kind of code requires a type annotation, because without an initial value being assigned Swift doesn’t know what kind of data username will contain.

Regardless of whether you use type inference or type annotation, there is one golden rule: Swift must at all times know what data types your constants and variables contain. This is at the core of being a type-safe language, and stops us doing nonsense things like 5 + true or similar.


##### Summary: Complex data

Arrays let us store lots of values in one place, then read them out using integer indices. Arrays must always be specialized so they contain one specific type, and they have helpful functionality such as count, append(), and contains().
Dictionaries also let us store lots of values in one place, but let us read them out using keys we specify. They must be specialized to have one specific type for key and another for the value, and have similar functionality to arrays, such as contains() and count.
Sets are a third way of storing lots of values in one place, but we don’t get to choose the order in which they store those items. Sets are really efficient at finding whether they contain a specific item.
Enums let us create our own simple types in Swift so that we can specify a range of acceptable values such as a list of actions the user can perform, the types of files we are able to write, or the types of notification to send to the user.
Swift must always know the type of data inside a constant or variable, and mostly uses type inference to figure that out based on the data we assign. However, it’s also possible to use type annotation to force a particular type.



