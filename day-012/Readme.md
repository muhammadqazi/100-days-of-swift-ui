# Day 012


##### How to create your own classes

Swift uses structs for storing most of its data types, including String, Int, Double, and Array, but there is another way to create custom data types called classes. These have many things in common with structs, but are different in key places.

First, the things that classes and structs have in common include:

- You get to create and name them.
- You can add properties and methods, including property observers and access control.
- You can create custom initializers to configure new instances however you want.

However, classes differ from structs in five key places:

1. You can make one class build upon functionality in another class, gaining all its properties and methods as a starting point. If you want to selectively override some methods, you can do that too.
2. Because of that first point, Swift won’t automatically generate a memberwise initializer for classes. This means you either need to write your own initializer, or assign default values to all your properties.
3. When you copy an instance of a class, both copies share the same data – if you change one copy, the other one also changes.
4. When the final copy of a class instance is destroyed, Swift can optionally run a special function called a deinitializer.
5. Even if you make a class constant, you can still change its properties as long as they are variables.

On the surface those probably seem fairly random, and there’s a good chance you’re probably wondering why classes are even needed when we already have structs.

However, SwiftUI uses classes extensively, mainly for point 3: all copies of a class share the same data. This means many parts of your app can share the same information, so that if the user changed their name in one screen all the other screens would automatically update to reflect that change.

The other points matter, but are of varying use:

1. Being able to build one class based on another is really important in Apple’s older UI framework, UIKit, but is much less common in SwiftUI apps. In UIKit it was common to have long class hierarchies, where class A was built on class B, which was built on class C, which was built on class D, etc.
2. Lacking a memberwise initializer is annoying, but hopefully you can see why it would be tricky to implement given that one class can be based upon another – if class C added an extra property it would break all the initializers for C, B, and A.
3. Being able to change a constant class’s variables links in to the multiple copy behavior of classes: a constant class means we can’t change what pot our copy points to, but if the properties are variable we can still change the data inside the pot. This is different from structs, where each copy of a struct is unique and holds its own data.
4. Because one instance of a class can be referenced in several places, it becomes important to know when the final copy has been destroyed. That’s where the deinitializer comes in: it allows us to clean up any special resources we allocated when that last copy goes away.

Before we’re done, let’s look at just a tiny slice of code that creates and uses a class:

```
class Game {
    var score = 0 {
        didSet {
            print("Score is now \(score)")
        }
    }
}

var newGame = Game()
newGame.score += 10
```

Yes, the only difference between that and a struct is that it was created using class rather than struct – everything else is identical. That might make classes seem redundant, but trust me: all five of their differences are important.

I’ll be going into more detail on the five differences between classes and structs in the following chapters, but right now the most important thing to know is this: structs are important, and so are classes – you will need both when using SwiftUI.



##### How to make one class inherit from another


Swift lets us create classes by basing them on existing classes, which is a process known as inheritance. When one class inherits functionality from another class (its “parent” or “super” class), Swift will give the new class access (the “child class” or “subclass”) to the properties and methods from that parent class, allowing us to make small additions or changes to customize the way the new class behaves.

To make one class inherit from another, write a colon after the child class’s name, then add the parent class’s name. For example, here is an Employee class with one property and an initializer:

```
class Employee {
    let hours: Int

    init(hours: Int) {
        self.hours = hours
    }
}
```

We could make two subclasses of Employee, each of which will gain the hours property and initializer:



```
class Developer: Employee {
    func work() {
        print("I'm writing code for \(hours) hours.")
    }
}

class Manager: Employee {
    func work() {
        print("I'm going to meetings for \(hours) hours.")
    }
}
```

Notice how those two child classes can refer directly to hours – it’s as if they added that property themselves, except we don’t have to keep repeating ourselves.

Each of those classes inherit from Employee, but each then adds their own customization. So, if we create an instance of each and call work(), we’ll get a different result:

```
let robert = Developer(hours: 8)
let joseph = Manager(hours: 10)
robert.work()
joseph.work()
```

As well as sharing properties, you can also share methods, which can then be called from the child classes. As an example, try adding this to the Employee class:

```
func printSummary() {
    print("I work \(hours) hours a day.")
}
```

##### How to add initializers for classes


super is another one of those values that Swift automatically provides for us, similar to self: it allows us to call up to methods that belong to our parent class, such as its initializer. You can use it with other methods if you want; it’s not limited to initializers.

Now that we have a valid initializer in both our classes, we can make an instance of Car like so

```
class Car: Vehicle {
    let isConvertible: Bool

    init(isElectric: Bool, isConvertible: Bool) {
        self.isConvertible = isConvertible
        super.init(isElectric: isElectric)
    }
}
```

##### Deinit

```
class User {
    let id: Int

    init(id: Int) {
        self.id = id
        print("User \(id): I'm alive!")
    }

    deinit {
        print("User \(id): I'm dead!")
    }
}
```