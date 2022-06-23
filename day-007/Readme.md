# Day 007


##### How to reuse code with functions

```
func showWelcome() {
    print("Welcome to my app!")
    print("By default This prints out a conversion")
    print("chart from centimeters to inches, but you")
    print("can also set a custom range if you want.")
}
```

- It starts with the func keyword, which marks the start of a function.
- We’re naming the function showWelcome. This can be any name you want, but try to make it memorable – printInstructions(), displayHelp(), etc are all good choices.
- The body of the function is contained within the open and close braces, just like the body of loops and the body of conditions.

There’s one extra thing in there, and you might recognize it from our work so far: the () directly after showWelcome. We first met these way back when we looked at strings, when I said that count doesn’t have () after it, but uppercased() does.

Well, now you’re learning why: those () are used with functions. They are used when you create the function, as you can see above, but also when you call the function – when you ask Swift to run its code. In our case, we can call our function like this:


As an example, we already used code like this:


```
let number = 139

if number.isMultiple(of: 2) {
    print("Even")
} else {
    print("Odd")
}
```

isMultiple(of:) is a function that belongs to integers. If it didn’t allow any kind of customization, it just wouldn’t make sense – is it a multiple of what? Sure, Apple could have made this be something like isOdd() or isEven() so it never needed to have configuration options, but by being able to write (of: 2) suddenly the function becomes more powerful, because now we can check for multiples of 2, 3, 4, 5, 50, or any other number.

Similarly, when we were rolling virtual dice earlier we used code like this:

```
let roll = Int.random(in: 1...20)
```

Again, random() is a function, and the (in: 1...20) part marks configuration options – without that we’d have no control over the range of our random numbers, which would be significantly less useful.

We can make our own functions that are open to configuration, all by putting extra code in between the parentheses when we create our function. This should be given a single integer, such as 8, and calculate the multiplication tables for that from 1 through 12.

Here’s that in code:


```
func printTimesTables(number: Int) {
    for i in 1...12 {
        print("\(i) x \(number) is \(i * number)")
    }
}

printTimesTables(number: 5)
```

Notice how I’ve placed number: Int inside the parentheses? That’s called a parameter, and it’s our customization point. We’re saying whoever calls this function must pass in an integer here, and Swift will enforce it. Inside the function, number is available to use like any other constant, so it appears inside the print() call.

As you can see, when printTimesTables() is called, we need to explicitly write number: 5 – we need to write the parameter name as part of the function call. This isn’t common in other languages, but I think it’s really helpful in Swift as a reminder of what each parameter does.

This naming of parameters becomes even more important when you have multiple parameters. For example, if we wanted to customize how high our multiplication tables went we might make the end of our range be set using a second parameter, like this:

```
func printTimesTables(number: Int, end: Int) {
    for i in 1...end {
        print("\(i) x \(number) is \(i * number)")
    }
}

printTimesTables(number: 5, end: 20)
```

##### How to return values from functions

If you want to return your own value from a function, you need to do two things:

- Write an arrow then a data type before your function’s opening brace, which tells Swift what kind of data will get sent back.
- Use the return keyword to send back your data.

For example, perhaps you want to roll a dice in various parts of your program, but rather than always forcing the dice roll to use a 6-sided dice you could instead make it a function:

```
func rollDice() -> Int {
    return Int.random(in: 1...6)
}

let result = rollDice()
print(result)
```


##### How to return multiple values from functions


If you want to return two or more values from a function, you could use an array. For example, here’s one that sends back a user’s details:

```
func getUser() -> [String] {
    ["Taylor", "Swift"]
}

let user = getUser()
print("Name: \(user[0]) \(user[1])") 
```

That’s problematic, because it’s hard to remember what user[0] and user[1] are, and if we ever adjust the data in that array then user[0] and user[1] could end up being something else or perhaps not existing at all.

We could use a dictionary instead, but that has its own problems:

```
func getUser() -> [String: String] {
    [
        "firstName": "Taylor",
        "lastName": "Swift"
    ]
}

let user = getUser()
print("Name: \(user["firstName", default: "Anonymous"]) \(user["lastName", default: "Anonymous"])") 
```

Yes, we’ve now given meaningful names to the various parts of our user data, but look at that call to print() – even though we know both firstName and lastName will exist, we still need to provide default values just in case things aren’t what we expect.

Both of these solutions are pretty bad, but Swift has a solution in the form of tuples. Like arrays, dictionaries, and sets, tuples let us put multiple pieces of data into a single variable, but unlike those other options tuples have a fixed size and can have a variety of data types.

Here’s how our function looks when it returns a tuple:

```
func getUser() -> (firstName: String, lastName: String) {
    (firstName: "Taylor", lastName: "Swift")
}

let user = getUser()
print("Name: \(user.firstName) \(user.lastName)")
```

In fact, if you don’t need all the values from the tuple you can go a step further by using _ to tell Swift to ignore that part of the tuple:

```
let (firstName, _) = getUser()
print("Name: \(firstName)")
```

##### How to customize parameter labels


You’ve seen how Swift developers like to name their function parameters, because it makes it easier to remember what they do when the function is called. For example, we could write a function to roll a dice a certain number of times, using parameters to control the number of sides on the dice and the number of rolls:

```
func rollDice(sides: Int, count: Int) -> [Int] {
    var rolls = [Int]()

    for _ in 1...count {
        let roll = Int.random(in: 1...sides)
        rolls.append(roll)
    }

    return rolls
}

let rolls = rollDice(sides: 6, count: 4)
```

Even if you came back to this code six months later, I feel confident that rollDice(sides: 6, count: 4) is pretty self-explanatory.

This method of naming parameters for external use is so important to Swift that it actually uses the names when it comes to figuring out which method to call. This is quite unlike many other languages, but this is perfect valid in Swift:



```
func hireEmployee(name: String) { }
func hireEmployee(title: String) { }
func hireEmployee(location: String) { }
```

Yes, those are all functions called hireEmployee(), but when you call them Swift knows which one you mean based on the parameter names you provide. To distinguish between the various options, it’s very common to see documentation refer to each function including its parameters, like this: hireEmployee(name:) or hireEmployee(title:).

Sometimes, though, these parameter names are less helpful, and there are two ways I want to look at.

First, think about the hasPrefix() function you learned earlier:

You might look at that and think it’s exactly right, but you might also look at string: string and see annoying duplication. After all, what else are we going to pass in there other than a string?

If we add an underscore before the parameter name, we can remove the external parameter label like so:

```
func isUppercase(_ string: String) -> Bool {
    string == string.uppercased()
}

let string = "HELLO, WORLD"
let result = isUppercase(string)
```

You already saw how we can put _ before the parameter name so that we don’t need to write an external parameter name. Well, the other option is to write a second name there: one to use externally, and one to use internally.

Here’s how that looks:

```
func printTimesTables(for number: Int) {
    for i in 1...12 {
        print("\(i) x \(number) is \(i * number)")
    }
}

printTimesTables(for: 5)
```

There are three things in there you need to look at closely:

- We write for number: Int: the external name is for, the internal name is number, and it’s of type Int.
- When we call the function we use the external name for the parameter: printTimesTables(for: 5).
- Inside the function we use the internal name for the parameter: print("\(i) x \(number) is \(i * number)").

So, Swift gives us two important ways to control parameter names: we can use _ for the external parameter name so that it doesn’t get used, or add a second name there so that we have both external and internal parameter names.


