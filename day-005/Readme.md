# Day 005



##### How to check a condition is true or false

Programs very often make choices:

- If the student’s exam score was over 80 then print a success message.
- If the user entered a name that comes after their friend’s name alphabetically, put the friend’s name first.
- If adding a number to an array makes it contain more than 3 items, remove the oldest one.
- If the user was asked to enter their name and typed nothing at all, give them a default name of “Anonymous”.

Swift handles these with if statements, which let us check a condition and run some code if the condition is true. They look like this:

```
if someCondition {
    print("Do something")
}
```

Let’s break that down:

- The condition starts with if, which signals to Swift we want to check some kind of condition in our code.
- The someCondition part is where you write your condition – was the score over 80? Does the array contain more than 3 items?
- If the condition is true – if the score really is over 80 – then we print the “Do something” message.

Of course, that isn’t everything in the code: I didn’t mention the little { and } symbols. These are called braces – opening and closing braces, more specifically – although sometimes you’ll hear them referred to as “curly braces” or “curly brackets”.

These braces are used extensively in Swift to mark blocks of code: the opening brace starts the block, and the closing brace ends it. Inside the code block is all the code we want to run if our condition happens to be true when it’s checked, which in our case is printing a message.

You can include as much code in there as you want:

```
if someCondition {
    print("Do something")
    print("Do something else")
    print("Do a third thing")
}
```

Let’s take a look at our third example condition: if adding a number to an array makes it contain more than 3 items, remove the oldest one. You’ve already met append(), count, and remove(at: ), so we can now put all three together with a condition like this:


```
var numbers = [1, 2, 3]

numbers.append(4)

if numbers.count > 3 {
    numbers.remove(at: 0)
}

print(numbers)
```

Swift adds a second piece of functionality to all its strings, arrays, dictionaries, and sets: isEmpty. This will send back true if the thing you’re checking has nothing inside, and we can use it to fix our condition like this:

```
if username.isEmpty == true {
    username = "Anonymous"
}
```

##### How does Swift let us compare many types of data?

We can even ask Swift to make our enums comparable, like this:


```
enum Sizes: Comparable {
    case small
    case medium
    case large
}

let first = Sizes.small
let second = Sizes.large
print(first < second)
```

##### How to check multiple conditions


```
enum TransportOption {
    case airplane, helicopter, bicycle, car, scooter
}

let transport = TransportOption.airplane

if transport == .airplane || transport == .helicopter {
    print("Let's fly!")
} else if transport == .bicycle {
    print("I hope there's a bike path…")
} else if transport == .car {
    print("Time to get stuck in traffic.")
} else {
    print("I'm going to hire a scooter now!")
}
```


- When we set the value for transport we need to be explicit that we’re referring to TransportOption.airplane. We can’t just write .airplane because Swift doesn’t understand we mean the TransportOption enum.
- Once that has happened, we don’t need to write TransportOption any more because Swift knows transport must be some kind of TransportOption. So, we can check whether it’s equal to .airplane rather than TransportOption.airplane.
- The code using || to check whether transport is equal to .airplane or equal to .helicopter, and if either of them are true then the condition is true, and “Let’s fly!” is printed.
- If the first condition fails – if the transport mode isn’t .airplane or .helicopter – then the second condition is run: is the transport mode .bicycle? If so, “I hope there’s a bike path…” is printed.
- If we aren’t going by bicycle either, then we check whether we’re going by car. If we are, “Time to get stuck in traffic.” is printed.
- Finally, if all the previous conditions fail then the else block is run, and it means we’re going by scooter.


##### How to use switch statements to check multiple conditions


```
switch forecast {
case .sun:
    print("It should be a nice day.")
case .rain:
    print("Pack an umbrella.")
case .wind:
    print("Wear something warm")
case .snow:
    print("School is cancelled.")
case .unknown:
    print("Our forecast generator is broken!")
}
```
That default: at the end is the default case, which will be run if all cases have failed to match.



Let’s break that down:

- We start with switch forecast, which tells Swift that’s the value we want to check.
- We then have a string of case statements, each of which are values we want to compare against forecast.
- Each of our cases lists one weather type, and because we’re switching on forecast we don’t need to write Weather.sun, Weather.rain and so on – Swift knows it must be some kind of Weather.
- After each case, we write a colon to mark the start of the code to run if that case is matched.
- We use a closing brace to end the switch statement.
- If you try changing .snow for .rain, you’ll see Swift complains loudly: once that we’ve checked .rain twice, and again that our switch statement is not exhaustive – that it doesn’t handle all possible cases.

If you’ve ever used other programming languages, you might have noticed that Swift’s switch statement is different in two places:

- All switch statements must be exhaustive, meaning that all possible values must be handled in there so you can’t leave one off by accident.
- Swift will execute the first case that matches the condition you’re checking, but no more. Other languages often carry on executing other code from all subsequent cases, which is usually entirely the wrong default thing to do.


We can also use fallthrough, this key word will go through all conditions beginning from the true one

```
let day = 5
print("My true love gave to me…")

switch day {
case 5:
    print("5 golden rings")
    fallthrough
case 4:
    print("4 calling birds")
    fallthrough
case 3:
    print("3 French hens")
    fallthrough
case 2:
    print("2 turtle doves")
    fallthrough
default:
    print("A partridge in a pear tree")
}
```

That will match the first case and print “5 golden rings”, but the fallthrough line means case 4 will execute and print “4 calling birds”, which in turn uses fallthrough again so that “3 French hens” is printed, and so on. It’s not a perfect match to the song, but at least you can see the functionality in action


##### How to use the ternary conditional operator for quick tests


```
let age = 18
let canVote = age >= 18 ? "Yes" : "No"
```

When that code runs, canVote will be set to “Yes” because age is set to 18.

As you can see, the ternary operator is split into three parts: a check (age >= 18), something for when the condition is true (“Yes”), and something for when the condition is false (“No”). That makes it exactly like a regular if and else block, in the same order.

If it helps, Scott Michaud suggested a helpful mnemonic: WTF. It stands for “what, true, false”, and matches the order of our code:

- What is our condition? Well, it’s age >= 18.
- What to do when the condition is true? Send back “Yes”, so it can be stored in canVote.
- And if the condition is false? Send back “No”.

Let’s look at some other examples, start with an easy one that reads an hour in 24-hour format and prints one of two messages:


```
let hour = 23
print(hour < 12 ? "It's before noon" : "It's after noon")
```