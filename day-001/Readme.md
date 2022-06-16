# Day 001


##### Why does Swift have variables?

Variables allow us to store temporary information in our program, and form a key part of almost every Swift program. Ultimately, your program is going to transform data somehow: maybe you let the user enter in todo list tasks then check them off, maybe you let them roam around a deserted island working for a capitalist raccoon, or maybe you read the device time and display it in a clock. Regardless, you’re taking some sort of data, transforming it somehow, and showing it to the user.

Of course, the “transforming it somehow” is where the real magic comes in, because that’s the part where your brilliant app idea happens. But the process of storing data in memory – holding on to something the user typed, or something you downloaded from the internet – is where variables come in.

Once you create a variable using var, you can change it as often as you want without using var again.

For Example:

```
var favoriteShow = "Orange is the New Black"
favoriteShow = "The Good Place"
favoriteShow = "Doctor Who"
```


If it helps, try reading var as “create a new variable”. So, the first line above might be read out loud as “create a new variable called favoriteShow and give it the value Orange is the New Black.” Lines 2 and 3 don’t have var in there, so they modify the existing value rather than creating a new variable.

Now imagine you had var on all three lines – you used var favoriteShow each time. That wouldn’t make much sense, because you’d be saying “create a new variable called favoriteShow” three times over, and the variable is clearly not new after your first attempt. Swift will flag this as an error, which means it won’t let you run your code until you pick a different name for your variables.

That might seem like annoying behavior, but trust me: it’s helpful! Swift wants you to be clear: are you trying to modify an existing variable (if so, remove the var the second and subsequent times), or are you trying to create a new variable (in which case, name it something else.)


##### Why does Swift have constants as well as variables?

Swift really loves constants, and in fact will recommend you use one if you created a variable then never changed its value. The reason for this is about avoiding problems: any variable you create can be changed by you whenever you want and as often as you want, so you lose some control – that important piece of user data you stashed away might be removed or replaced at any point in the future.

Constants don’t let us change values once they are set, so it’s a bit like a contract with Swift: you’re saying “this value matters, don’t let me change it no matter what I do.” Sure, you could try to make the same contract with a variable, but one slip of your keyboard could screw things up and Swift wouldn’t be able to help. By using a constant instead – just by changing var to let – you’re asking Swift to make sure the value never changes, which is another thing you no longer need to worry about.


##### Multi-line strings

```
var quote = "Change the world by being yourself"

```

```
var burns = """
The best laid schemes
O’ mice and men
Gang aft agley
"""
```

