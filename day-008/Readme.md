# Day 008


##### How to provide default values for parameters

Swift lets us specify default values for any or all of our parameters. In this case, we could set end to have the default value of 12, meaning that if we don’t specify it 12 will be used automatically.

Here’s how that looks in code:

```
func printTimesTables(for number: Int, end: Int = 12) {
    for i in 1...end {
        print("\(i) x \(number) is \(i * number)")
    }
}

printTimesTables(for: 5, end: 20)
printTimesTables(for: 8)
```

##### How to handle errors in functions

Things go wrong all the time, such as when that file you wanted to read doesn’t exist, or when that data you tried to download failed because the network was down. If we didn’t handle errors gracefully then our code would crash, so Swift makes us handle errors – or at least acknowledge when they might happen.

This takes three steps:

Telling Swift about the possible errors that can happen.
Writing a function that can flag up errors if they happen.
Calling that function, and handling any errors that might happen.
Let’s work through a complete example: if the user asks us to check how strong their password is, we’ll flag up a serious error if the password is too short or is obvious.

So, we need to start by defining the possible errors that might happen. This means making a new enum that builds on Swift’s existing Error type, like this:


```
enum PasswordError: Error {
    case short, obvious
}
```

That says there are two possible errors with password: short and obvious. It doesn’t define what those mean, only that they exist.

Step two is to write a function that will trigger one of those errors. When an error is triggered – or thrown in Swift – we’re saying something fatal went wrong with the function, and instead of continuing as normal it immediately terminates without sending back any value.

In our case, we’re going to write a function that checks the strength of a password: if it’s really bad – fewer than 5 characters or is extremely well known – then we’ll throw an error immediately, but for all other strings we’ll return either “OK”, “Good”, or “Excellent” ratings.

Here’s how that looks in Swift:


```
func checkPassword(_ password: String) throws -> String {
    if password.count < 5 {
        throw PasswordError.short
    }

    if password == "12345" {
        throw PasswordError.obvious
    }

    if password.count < 8 {
        return "OK"
    } else if password.count < 10 {
        return "Good"
    } else {
        return "Excellent"
    }
}
```

Let’s break that down…

- If a function is able to throw errors without handling them itself, you must mark the function as throws before the return type.
- We don’t specify exactly what kind of error is thrown by the function, just that it can throw errors.
- Being marked with throws does not mean the function must throw errors, only that it might.
- When it comes time to throw an error, we write throw followed by one of our PasswordError cases. This immediately exits the function, meaning that it won’t return a string.
- If no errors are thrown, the function must behave like normal – it needs to return a string.

That completes the second step of throwing errors: we defined the errors that might happen, then wrote a function using those errors.

The final step is to run the function and handle any errors that might happen. Swift Playgrounds are pretty lax about error handling because they are mostly meant for learning, but when it comes to working with real Swift projects you’ll find there are three steps:

- Starting a block of work that might throw errors, using do.
- Calling one or more throwing functions, using try.
- Handling any thrown errors using catch.

In pseudocode, it looks like this:

```
do {
    try someRiskyWork()
} catch {
    print("Handle errors here")
}
```

If we wanted to write try that using our current checkPassword() function, we could write this:



```
let string = "12345"

do {
    let result = try checkPassword(string)
    print("Password rating: \(result)")
} catch {
    print("There was an error.")
}
```

If the checkPassword() function works correctly, it will return a value into result, which is then printed out. But if any errors are thrown (which in this case there will be), the password rating message will never be printed – execution will immediately jump to the catch block.

There are a few different parts of that code that deserve discussion, but I want to focus on the most important one: try. This must be written before calling all functions that might throw errors, and is a visual signal to developers that regular code execution will be interrupted if an error happens.

When you use try, you need to be inside a do block, and make sure you have one or more catch blocks able to handle any errors. In some circumstances you can use an alternative written as try! which does not require do and catch, but will crash your code if an error is thrown – you should use this rarely, and only if you’re absolutely sure an error cannot be thrown.

When it comes to catching errors, you must always have a catch block that is able to handle every kind of error. However, you can also catch specific errors as well, if you want:

```
let string = "12345"

do {
    let result = try checkPassword(string)
    print("Password rating: \(result)")
} catch PasswordError.short {
    print("Please use a longer password.")
} catch PasswordError.obvious {
    print("I have the same combination on my luggage!")
} catch {
    print("There was an error.")
}
```

##### Tip: 
Most errors thrown by Apple provide a meaningful message that you can present to your user if needed. Swift makes this available using an error value that’s automatically provided inside your catch block, and it’s common to read error.localizedDescription to see exactly what happened.




