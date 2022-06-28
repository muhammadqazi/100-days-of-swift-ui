# Day 011


##### How to limit access to internal data using access control

By default, Swift’s structs let us access their properties and methods freely, but often that isn’t what you want – sometimes you want to hide some data from external access. For example, maybe you have some logic you need to apply before touching your properties, or maybe you know that some methods need to be called in a certain way or order, and so shouldn’t be touched externally.

We can demonstrate the problem with an example struct:

```
struct BankAccount {
    var funds = 0

    mutating func deposit(amount: Int) {
        funds += amount
    }

    mutating func withdraw(amount: Int) -> Bool {
        if funds >= amount {
            funds -= amount
            return true
        } else {
            return false
        }
    }
}
```

That has methods to deposit and withdraw money from a bank account, and should be used like this:


```
var account = BankAccount()
account.deposit(amount: 100)
let success = account.withdraw(amount: 200)

if success {
    print("Withdrew money successfully")
} else {
    print("Failed to get the money")
}
```

But the funds property is just exposed to us externally, so what’s stopping us from touching it directly? The answer is nothing at all – this kind of code is allowed:

```
private var funds = 0
```

And now accessing funds from outside the struct isn’t possible, but it is possible inside both deposit() and withdraw(). If you try to read or write funds from outside the struct Swift will refuse to build your code.

This is called access control, because it controls how a struct’s properties and methods can be accessed from outside the struct.

Swift provides us with several options, but when you’re learning you’ll only need a handful:

Use private for “don’t let anything outside the struct use this.”
Use fileprivate for “don’t let anything outside the current file use this.”
Use public for “let anyone, anywhere use this.”
There’s one extra option that is sometimes useful for learners, which is this: private(set). This means “let anyone read this property, but only let my methods write it.” If we had used that with BankAccount, it would mean we could print account.funds outside of the struct, but only deposit() and withdraw() could actually change the value.

In this case, I think private(set) is the best choice for funds: you can read the current bank account balance at any time, but you can’t change it without running through my logic.

If you think about it, access control is really about limiting what you and other developers on your team are able to do – and that’s sensible! If we can make Swift itself stop us from making mistakes, that’s always a smart move.

Important: If you use private access control for one or more properties, chances are you’ll need to create your own initializer.

##### Static properties and methods


You’ve seen how we can attach properties and methods to structs, and how each struct has its own unique copy of those properties so that calling a method on the struct won’t read the properties of a different struct from the same type.

Well, sometimes – only sometimes – you want to add a property or method to the struct itself, rather than to one particular instance of the struct, which allows you to use them directly. I use this technique a lot with SwiftUI for two things: creating example data, and storing fixed data that needs to be accessed in various places.

First, let’s look at a simplified example of how to create and use static properties and methods:

```
struct School {
    static var studentCount = 0

    static func add(student: String) {
        print("\(student) joined the school.")
        studentCount += 1
    }
}
```

Notice the keyword static in there, which means both the studentCount property and add() method belong to the School struct itself, rather than to individual instances of the struct.

To use that code, we’d write the following:



```
School.add(student: "Taylor Swift")
print(School.studentCount)
```

I haven’t created an instance of School – we can literally use add() and studentCount directly on the struct. This is because those are both static, which means they don’t exist uniquely on instances of the struct.

This should also explain why we’re able to modify the studentCount property without marking the method as mutating – that’s only needed with regular struct functions for times when an instance of struct was created as a constant, and there is no instance when calling add().

If you want to mix and match static and non-static properties and methods, there are two rules:

To access non-static code from static code… you’re out of luck: static properties and methods can’t refer to non-static properties and methods because it just doesn’t make sense – which instance of School would you be referring to?
To access static code from non-static code, always use your type’s name such as School.studentCount. You can also use Self to refer to the current type.
Now we have self and Self, and they mean different things: self refers to the current value of the struct, and Self refers to the current type.

Tip: It’s easy to forget the difference between self and Self, but if you think about it it’s just like the rest of Swift’s naming – we start all our data types with a capital letter (Int, Double, Bool, etc), so it makes sense for Self to start with a capital letter too.

Now, that sound you can hear is a thousand other learners saying “why the heck is this needed?” And I get it – this can seem like a rather redundant feature at first. So, I want to show you the two main ways I use static data.

First, I use static properties to organize common data in my apps. For example, I might have a struct like AppData to store lots of shared values I use in many places:

```
struct AppData {
    static let version = "1.3 beta 2"
    static let saveFilename = "settings.json"
    static let homeURL = "https://www.hackingwithswift.com"
}
```

Using this approach, everywhere I need to check or display something like my app’s version number – an about screen, debug output, logging information, support emails, etc – I can read AppData.version.

The second reason I commonly use static data is to create examples of my structs. As you’ll see later on, SwiftUI works best when it can show previews of your app as you develop, and those previews often require sample data. For example, if you’re showing a screen that displays data on one employee, you’ll want to be able to show an example employee in the preview screen so you can check it all looks correct as you work.

This is best done using a static example property on the struct, like this:
```
struct Employee {
    let username: String
    let password: String

    static let example = Employee(username: "cfederighi", password: "hairforceone")
}
```
And now whenever you need an Employee instance to work with in your design previews, you can use Employee.example and you’re done.

Like I said at the beginning, there are only a handful of occasions when a static property or method makes sense, but they are still a useful tool to have around.