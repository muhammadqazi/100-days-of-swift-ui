# Day 013


##### How to create and use protocols


Protocols are a bit like contracts in Swift: they let us define what kinds of functionality we expect a data type to support, and Swift ensures that the rest of our code follows those rules.


```
protocol Vehicle {
    func estimateTime(for distance: Int) -> Int
    func travel(distance: Int)
}
```

Let’s break that down:

- To create a new protocol we write protocol followed by the protocol name. This is a new type, so we need to use camel case starting with an uppercase letter.
- Inside the protocol we list all the methods we require in order for this protocol to work the way we expect.
- These methods do not contain any code inside – there are no function bodies provided here. Instead, we’re specifying the method names, parameters, and return types. You can also mark methods as being throwing or mutating if needed.

we could make a Car struct that conforms to Vehicle, like this:

```
struct Car: Vehicle {
    func estimateTime(for distance: Int) -> Int {
        distance / 50
    }

    func travel(distance: Int) {
        print("I'm driving \(distance)km.")
    }

    func openSunroof() {
        print("It's a nice day!")
    }
}
```


here are a few things I want to draw particular attention to in that code:

- We tell Swift that Car conforms to Vehicle by using a colon after the name Car, just like how we mark subclasses.
- All the methods we listed in Vehicle must exist exactly in Car. If they have slightly different names, accept different parameters, have different return types, etc, then Swift will say we haven’t conformed to the protocol.
- The methods in Car provide actual implementations of the methods we defined in the protocol. In this case, that means our struct provides a rough estimate for how many minutes it takes to drive a certain distance, and prints a message when travel() is called.
- The openSunroof() method doesn’t come from the Vehicle protocol, and doesn’t really make sense there because many vehicle types don’t have a sunroof. But that’s okay, because the protocol describes only the minimum functionality conforming types must have, and they can add their own as needed.

So, now we’ve created a protocol, and made a Car struct that conforms to the protocol.

To finish off, let’s update the commute() function from earlier so that it uses the new methods we added to Car:

```
func commute(distance: Int, using vehicle: Car) {
    if vehicle.estimateTime(for: distance) > 100 {
        print("That's too slow! I'll try a different vehicle.")
    } else {
        vehicle.travel(distance: distance)
    }
}

let car = Car()
commute(distance: 100, using: car)
```

##### How to create and use extensions


```
extension String {
    func trimmed() -> String {
        self.trimmingCharacters(in: .whitespacesAndNewlines)
    }
}
```

- We start with the extension keyword, which tells Swift we want to add functionality to an existing type.
- Which type? Well, that comes next: we want to add functionality to String.
- Now we open a brace, and all the code until the final closing brace is there to be added to strings.
- We’re adding a new method called trimmed(), which returns a new string.
- Inside there we call the same method as before: trimmingCharacters(in:), sending back its result.

Notice how we can use self here – that automatically refers to the current string. This is possible because we’re currently in a string extension.