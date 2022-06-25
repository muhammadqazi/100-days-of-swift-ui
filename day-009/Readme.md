# Day 009



##### How to create and use closures

Functions are powerful things in Swift. Yes, you’ve seen how you can call them, pass in values, and return data, but you can also assign them to variables, pass functions into functions, and even return functions from functions.

For example:

```
func greetUser() {
    print("Hi there!")
}

greetUser()

var greetCopy = greetUser
greetCopy()
```

Swift gives this the grandiose name closure expression, which is a fancy way of saying we just created a closure – a chunk of code we can pass around and call whenever we want. This one doesn’t have a name, but otherwise it’s effectively a function that takes no parameters and doesn’t return a value.

If you want the closure to accept parameters, they need to be written in a special way. You see, the closure starts and ends with the braces, which means we can’t put code outside those braces to control parameters or return value. So, Swift has a neat workaround: we can put that same information inside the braces, like this:

```
let sayHello = { (name: String) -> String in
    "Hi \(name)!"
}
```
I added an extra keyword there – did you spot it? It’s the in keyword, and it comes directly after the parameters and return type of the closure. Again, with a regular function the parameters and return type would come outside the braces, but we can’t do that with closures. So, in is used to mark the end of the parameters and return type – everything after that is the body of the closure itself. There’s a reason for this, and you’ll see it for yourself soon enough.

In the meantime, you might have a more fundamental question: “why would I ever need these things?” I know, closures do seem awfully obscure. Worse, they seem obscure and complicated – many, many people really struggle with closures when they first meet them, because they are complex beasts and seem like they are never going to be useful.

However, as you’ll see this gets used extensively in Swift, and almost everywhere in SwiftUI. Seriously, you’ll use them in every SwiftUI app you write, sometimes hundreds of times – maybe not necessarily in the form you see above, but you’re going to be using it a lot.

To get an idea of why closures are so useful, I first want to introduce you to function types. You’ve seen how integers have the type Int, and decimals have the type Double, etc, and now I want you to think about how functions have types too.

Let’s take the greetUser() function we wrote earlier: it accepts no parameters, returns no value, and does not throw errors. If we were to write that as a type annotation for greetCopy, we’d write this:

```
var greetCopy: () -> Void = greetUser
```

Let’s break that down…

The empty parentheses marks a function that takes no parameters.
The arrow means just what it means when creating a function: we’re about to declare the return type for the function.
Void means “nothing” – this function returns nothing. Sometimes you might see this written as (), but we usually avoid that because it can be confused with the empty parameter list.
Every function’s type depends on the data it receives and sends back. That might sound simple, but it hides an important catch: the names of the data it receives are not part of the function’s type.

We can demonstrate this with some more code:

```
func getUserData(for id: Int) -> String {
    if id == 1989 {
        return "Taylor Swift"
    } else {
        return "Anonymous"
    }
}

let data: (Int) -> String = getUserData
let user = data(1989)
print(user)
```

Let’s write some new code that calls sorted() using a closure:

```
let captainFirstTeam = team.sorted(by: { (name1: String, name2: String) -> Bool in
    if name1 == "Suzanne" {
        return true
    } else if name2 == "Suzanne" {
        return false
    }

    return name1 < name2
})
```

That’s a big chunk of syntax all at once, and again I want to say it’s going to get easier – in the very next chapter we’re going to look at ways to reduce the amount of code so it’s easier to see what’s going on.

But first I want to break down what’s happening there:

- We’re calling the sorted() function as before.
- Rather than passing in a function, we’re passing a closure – everything from the opening brace after by: down to the closing brace on the last line is part of the closure.
- Directly inside the closure we list the two parameters sorted() will pass us, which are two strings. We also say that our closure will return a Boolean, then mark the start of the closure’s code by using in.
- Everything else is just normal function code.

Again, there’s a lot of syntax in there and I wouldn’t blame you if you felt a headache coming on, but I hope you can see the benefit of closures at least a little: functions like sorted() let us pass in custom code to adjust how they work, and do so directly – we don’t need to write out a new function just for that one usage.


##### How to use trailing closures and shorthand syntax


Swift has a few tricks up its sleeve to reduce the amount of syntax that comes with closures, but first let’s remind ourselves of the problem. Here’s the code we ended up with at the end of the previous chapter:

If you remember, sorted() can accept any kind of function to do custom sorting, with one rule: that function must accept two items from the array in question (that’s two strings here), and return a Boolean set to true if the first string should be sorted before the second.

To be clear, the function must behave like that – if it returned nothing, or if it only accepted one string, then Swift would refuse to build our code.

Think it through: in this code, the function we provide to sorted() must provide two strings and return a Boolean, so why do we need to repeat ourselves in our closure?

The answer is: we don’t. We don’t need to specify the types of our two parameters because they must be strings, and we don’t need to specify a return type because it must be a Boolean. So, we can rewrite the code to this:

```
let captainFirstTeam = team.sorted { name1, name2 in
    if name1 == "Suzanne" {
        return true
    } else if name2 == "Suzanne" {
        return false
    }

    return name1 < name2
}
```

Rather than passing the closure in as a parameter, we just go ahead and start the closure directly – and in doing so remove (by: from the start, and a closing parenthesis at the end. Hopefully you can now see why the parameter list and in come inside the closure, because if they were outside it would look even weirder!

There’s one last way Swift can make closures less cluttered: Swift can automatically provide parameter names for us, using shorthand syntax. With this syntax we don’t even write name1, name2 in any more, and instead rely on specially named values that Swift provides for us: $0 and $1, for the first and second strings respectively.

Using this syntax our code becomes even shorter:

```
let captainFirstTeam = team.sorted {
    if $0 == "Suzanne" {
        return true
    } else if $1 == "Suzanne" {
        return false
    }

    return $0 < $1
}
```

So, in is used to mark the end of the parameters and return type – everything after that is the body of the closure itself. There’s a reason for this, and you’ll see it for yourself soon enough.

There I’ve flipped the comparison from < to > so we get a reverse sort, but now that we’re down to a single line of code we can remove the return and get it down to almost nothing:
```
let reverseTeam = team.sorted { $0 > $1 }
```

##### How to accept functions as parameters


I’ve added the type annotation in there intentionally, because that’s exactly what we use when specifying functions as parameters: we tell Swift what parameters the function accepts, as well its return type.

Once again, brace yourself: the syntax for this is a little hard on the eyes at first! Here’s a function that generates an array of integers by repeating a function a certain number of times:

```
func makeArray(size: Int, using generator: () -> Int) -> [Int] {
    var numbers = [Int]()

    for _ in 0..<size {
        let newNumber = generator()
        numbers.append(newNumber)
    }

    return numbers
}
```

Let’s break that down…

- The function is called makeArray(). It takes two parameters, one of which is the number of integers we want, and also returns an array of integers.
- The second parameter is a function. This accepts no parameters itself, but will return one integer every time it’s called.
- Inside makeArray() we create a new empty array of integers, then loop as many times as requested.
- Each time the loop goes around we call the generator function that was passed in as a parameter. This will return one new integer, so we put that into the numbers array.
- Finally the finished array is returned.
- The body of the makeArray() is mostly straightforward: repeatedly call a function to generate an integer, adding each value to an array, then send it all back.

The complex part is the very first line:

```
func makeArray(size: Int, using generator: () -> Int) -> [Int] {
```

While you’re learning Swift and SwiftUI, there will only be a handful of times when you need to know how to accept functions as parameters, but at least now you have an inkling of how it works and why it matters.

There’s one last thing before we move on: you can make your function accept multiple function parameters if you want, in which case you can specify multiple trailing closures. The syntax here is very common in SwiftUI, so it’s important to at least show you a taste of it here.

To demonstrate this here’s a function that accepts three function parameters, each of which accept no parameters and return nothing:

```
func doImportantWork(first: () -> Void, second: () -> Void, third: () -> Void) {
    print("About to start first work")
    first()
    print("About to start second work")
    second()
    print("About to start third work")
    third()
    print("Done!")
}
```

I’ve added extra print() calls in there to simulate specific work being done in between first, second, and third being called.

When it comes to calling that, the first trailing closure is identical to what we’ve used already, but the second and third are formatted differently: you end the brace from the previous closure, then write the external parameter name and a colon, then start another brace.

Here’s how that looks:


```
doImportantWork {
    print("This is the first work")
} second: {
    print("This is the second work")
} third: {
    print("This is the third work")
}
```

Having three trailing closures is not as uncommon as you might expect. For example, making a section of content in SwiftUI is done with three trailing closures: one for the content itself, one for a head to be put above, and one for a footer to be put below.

