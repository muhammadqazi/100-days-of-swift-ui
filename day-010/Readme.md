# Day 010



##### How to create your own structs

Swift’s structs let us create our own custom, complex data types, complete with their own variables and their own functions.

A simple struct looks like this:

```
struct Album {
    let title: String
    let artist: String
    let year: Int

    func printSummary() {
        print("\(title) (\(year)) by \(artist)")
    }
}
```

That creates a new type called Album, with two string constants called title and artist, plus an integer constant called year. I also added a simple function that summarizes the values of our three constants.

Notice how Album starts with a capital A? That’s the standard in Swift, and we’ve been using it all along – think of String, Int, Bool, Set, and so on. When you’re referring to a data type, we use camel case where the first letter is uppercased, but when you’re referring to something inside the type, such as a variable or function, we use camel case where the first letter is lowercased. Remember, for the most part this is only a convention rather than a rule, but it’s a helpful one to stick with.

At this point, Album is just like String or Int – we can make them, assign values, copy them, and so on. For example, we could make a couple of albums, then print some of their values and call their functions:



```
let red = Album(title: "Red", artist: "Taylor Swift", year: 2012)
let wings = Album(title: "Wings", artist: "BTS", year: 2016)

print(red.title)
print(wings.artist)

red.printSummary()
wings.printSummary()
```

Where things get more interesting is when you want to have values that can change. For example, we could create an Employee struct that can take vacation as needed:

```
struct Employee {
    let name: String
    var vacationRemaining: Int

    func takeVacation(days: Int) {
        if vacationRemaining > days {
            vacationRemaining -= days
            print("I'm going on vacation!")
            print("Days remaining: \(vacationRemaining)")
        } else {
            print("Oops! There aren't enough days remaining.")
        }
    }
}
```

However, that won’t actually work – Swift will refuse to build the code.

You see, if we create an employee as a constant using let, Swift makes the employee and all its data constant – we can call functions just fine, but those functions shouldn’t be allowed to change the struct’s data because we made it constant.

As a result, Swift makes us take an extra step: any functions that only read data are fine as they are, but any that change data belonging to the struct must be marked with a special mutating keyword, like this:

```
mutating func takeVacation(days: Int) {
```


##### How to compute property values dynamically


We could adjust this to use computed property, like so:


```
struct Employee {
    let name: String
    var vacationAllocated = 14
    var vacationTaken = 0

    var vacationRemaining: Int {
        vacationAllocated - vacationTaken
    }
}
```

This is really powerful stuff: we’re reading what looks like a property, but behind the scenes Swift is running some code to calculate its value every time.

We can’t write to it, though, because we haven’t told Swift how that should be handled. To fix that, we need to provide both a getter and a setter – fancy names for “code that reads” and “code that writes” respectively.

In this case the getter is simple enough, because it’s just our existing code. But the setter is more interesting – if you set vacationRemaining for an employee, do you mean that you want their vacationAllocated value to be increased or decreased, or should vacationAllocated stay the same and instead we change vacationTaken?

I’m going to assume the first of those two is correct, in which case here’s how the property would look:

```
var vacationRemaining: Int {
    get {
        vacationAllocated - vacationTaken
    }

    set {
        vacationAllocated = vacationTaken + newValue
    }
}
```

##### How to take action when a property changes

Swift lets us create property observers, which are special pieces of code that run when properties change. These take two forms: a didSet observer to run when the property just changed, and a willSet observer to run before the property changed.

If you want it, Swift automatically provides the constant oldValue inside didSet, in case you need to have custom functionality based on what you were changing from. There’s also a willSet variant that runs some code before the property changes, which in turn provides the new value that will be assigned in case you want to take different action based on that.

We can demonstrate all this functionality in action using one code sample, which will print messages as the values change so you can see the flow when the code is run:


```
struct App {
    var contacts = [String]() {
        willSet {
            print("Current value is: \(contacts)")
            print("New value will be: \(newValue)")
        }

        didSet {
            print("There are now \(contacts.count) contacts.")
            print("Old value was \(oldValue)")
        }
    }
}

var app = App()
app.contacts.append("Adrian E")
app.contacts.append("Allen W")
app.contacts.append("Ish S")
```

Yes, appending to an array will trigger both willSet and didSet, so that code will print lots of text when run.

In practice, willSet is used much less than didSet, but you might still see it from time to time so it’s important you know it exists. Regardless of which you choose, please try to avoid putting too much work into property observers – if something that looks trivial such as game.score += 1 triggers intensive work, it will catch you out on a regular basis and cause all sorts of performance problems.


##### How to create custom initializers


Initializers are specialized methods that are designed to prepare a new struct instance to be used. You’ve already seen how Swift silently generates one for us based on the properties we place inside a struct, but you can also create your own as long as you follow one golden rule: all properties must have a value by the time the initializer ends.

Let’s start by looking again at Swift’s default initializer for structs:

```
struct Player {
    let name: String
    let number: Int

    init(name: String, number: Int) {
        self.name = name
        self.number = number
    }
}
```

That works the same as our previous code, except now the initializer is owned by us so we can add extra functionality there if needed.

However, there are a couple of things I want you to notice:

- There is no func keyword. Yes, this looks like a function in terms of its syntax, but Swift treats initializers specially.
- Even though this creates a new Player instance, initializers never explicitly have a return type – they always return the type of data they belong to.
- I’ve used self to assign parameters to properties to clarify we mean “assign the name parameter to my name property”.

That last point is particularly important, because without self we’d have name = name and that doesn’t make sense – are we assigning the property to the parameter, assigning the parameter to itself, or something else? By writing self.name we’re clarifying we mean “the name property that belongs to my current instance,” as opposed to anything else.

Of course, our custom initializers don’t need to work like the default memberwise initializer Swift provides us with. For example, we could say that you must provide a player name, but the shirt number is randomized:



```
struct Player {
    let name: String
    let number: Int

    init(name: String) {
        self.name = name
        number = Int.random(in: 1...99)
    }
}

let player = Player(name: "Megan R")
print(player.number)
```
Just remember the golden rule: all properties must have a value by the time the initializer ends. If we had not provided a value for number inside the initializer, Swift would refuse to build our code.

Important: Although you can call other methods of your struct inside your initializer, you can’t do so before assigning values to all your properties – Swift needs to be sure everything is safe before doing anything else.

You can add multiple initializers to your structs if you want, as well as leveraging features such as external parameter names and default values. However, as soon as you implement your own custom initializers you’ll lose access to Swift’s generated memberwise initializer unless you take extra steps to retain it. This isn’t an accident: if you have a custom initializer, Swift effectively assumes that’s because you have some special way to initialize your properties, which means the default one should no longer be available.


