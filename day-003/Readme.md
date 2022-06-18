# Day 003


It’s extremely common to want to have lots of data in a single place, whether that’s the days of the week, a list of students in a class, a city’s population for the last 100 years, or any of countless other examples.

In Swift, we do this grouping using an array. Arrays are their own data type just like String, Int, and Double, but rather than hold just one string they can hold zero strings, one string, two strings, three, fifty, fifty million, or even more strings – they can automatically adapt to hold as many as you need, and always hold data in the order you add it.

Let’s start with some simple examples of creating arrays

##### Why does Swift have arrays?

```
var beatles = ["John", "Paul", "George", "Ringo"]
let numbers = [4, 8, 15, 16, 23, 42]
var temperatures = [25.3, 28.2, 26.4]
```
That creates three different arrays: one holding strings of people’s names, one holding integers of important numbers, and one holding decimals of temperatures in Celsius. Notice how we start and end arrays using square brackets, with commas between every item.

When it comes to reading values out from an array, we ask for values by the position they appear in the array. The position of an item in an array is commonly called its index.

This confuses beginners a bit, but Swift actually counts an item’s index from zero rather than one – beatles[0] is the first element, and beatles[1] is the second, for example.

So, we could read some values out from our arrays like this

```
print(beatles[0])
print(numbers[1])
print(temperatures[2])
```

If your array is variable, you can modify it after creating it. For example, you can use append() to add new items

```
beatles.append("Allen")
beatles.append("Adrian")
beatles.append("Novall")
beatles.append("Vivian")
```

You can see this more clearly when you want to start with an empty array and add items to it one by one. This is done with very precise syntax
```
var scores = Array<Int>()
scores.append(100)
scores.append(80)
scores.append(85)
print(scores[1])
```

Arrays are so common in Swift that there’s a special way to create them: rather than writing Array<String>, you can instead write [String]. So, this kind of code is exactly the same as before

```
var albums = [String]()
albums.append("Folklore")
albums.append("Fearless")
albums.append("Red")
```

You can use .count to read how many items are in an array, just like you did with strings
```
print(albums.count)
var characters = ["Lana", "Pam", "Ray", "Sterling"]
print(characters.count)
```

also, You can remove items from an array by using either remove(at: ) to remove one item at a specific index, or removeAll() to remove everything

```
characters.remove(at: 2)
print(characters.count)
```

You can check whether an array contains a particular item by using contains(), like this

```
characters.removeAll()
print(characters.count)
let bondMovies = ["Casino Royale", "Spectre", "No Time To Die"]
print(bondMovies.contains("Frozen"))
```


##### How to store and find data in dictionaries

Dictionaries don’t store items according to their position like arrays do, but instead let us decide where items should be stored.

For example, we could rewrite our previous example to be more explicit about what each item is

```
let employee2 = ["name": "Taylor Swift", "job": "Singer", "location": "Nashville"]
```

When it comes to reading data out from the dictionary, you use the same keys you used when creating it:

```
print(employee2["name"])
print(employee2["job"])
print(employee2["location"])
```

You can provide a default value to use if the key doesn’t exist

```
print(employee2["name", default: "Unknown"])
print(employee2["job", default: "Unknown"])
print(employee2["location", default: "Unknown"])
```

You can also create an empty dictionary using whatever explicit types you want to store, then set keys one by one

```
var heights = [String: Int]()
heights["Yao Ming"] = 229
heights["Shaquille O'Neal"] = 216
heights["LeBron James"] = 206
```

Finally, just like arrays and the other data types we’ve seen so far, dictionaries come with some useful functionality that you’ll want to use in the future – count and removeAll() both exists for dictionaries, and work just like they do for arrays

##### How to use sets for fast data lookup

```
let people = Set(["Denzel Washington", "Tom Cruise", "Nicolas Cage", "Samuel L Jackson"])
```

##### How to create and use enums


An enum – short for enumeration – is a set of named values we can create and use in our code. They don’t have any special meaning to Swift, but they are more efficient and safer, so you’ll use them a lot in your code

```
enum Weekday {
    case monday
    case tuesday
    case wednesday
    case thursday
    case friday
}
```


Swift does two things that make enums a little easier to use. First, when you have many cases in an enum you can just write case once, then separate each case with a comma

```
enum Weekday {
    case monday, tuesday, wednesday, thursday, friday
}
```

Remember that once you assign a value to a variable or constant, its data type becomes fixed – you can’t set a variable to a string at first, then an integer later on. Well, for enums this means you can skip the enum name after the first assignment, like this:

```
var day = Weekday.monday
day = .tuesday
day = .friday
```

Swift knows that .tuesday must refer to Weekday.tuesday because day must always be some kind of Weekday.

Although it isn’t visible here, one major benefit of enums is that Swift stores them in an optimized form – when we say Weekday.monday Swift is likely to store that using a single integer such as 0, which is much more efficient to store and check than the letters M, o, n, d, a, y


