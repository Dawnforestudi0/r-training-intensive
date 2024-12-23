---
title: Programming Essentials
--- 

In this workshop we cover the building blocks for developing more complex code, looking at

* Understanding variable types and methods
* Conditionals
* Loops
* Custom functions and modules

## Directing traffic with conditionals
In the first half of this session we'll look at two types of control flows: **conditionals** and **loops**.

Conditionals allow you to put "gates" in your code, only running sections if a certain **condition** is true. They are common to most programming languages.

In Python, they are called `if` statements, because you use the `if` command. For example,

```R
if (5 > 0) {
  print("We're inside the if statement")
}
```

The line `print("We're inside the if statement")` **will only run if `5 > 0` is true**. If not, it'll get skipped.

Curly brackets are essential. Only code inside them will be governed by conditional

```R
if (5 > 0) {
  print("We're inside the if statement")
}

print("This code always runs")
```

Watch what happens if we change the condition

```R
if (5 > 10) {
  print("We're inside the if statement")
}

print("This code always runs")
```

Now, the first line **doesn't run**. That's the essence of a conditional.

There's not much point to using a condition that will always be true. Typically, you'd use a variable in the condition, for example.

```R
pet_age <- 10

if (pet_age > 10) {
  print("My pet is older than 10")
}
```

### Logical operators

Here is a table of the different operators you can make conditions with. When you run them, they always return either `True` or `False`. 

| Operator | True example | Description |
| --- | --- | --- |
| `==` | `10 == 10` | Same value and type |
| `!=` | `"10" != 10` | Different value **or** type |
| `>` | `10 > 5` | Greater than |
| `>=` | `10 >= 10` | Greater than or equal to |
| `<` | `5 < 10` | Less than |
| `<=` | `5 <= 10` | Less than or equal to |
| `&&` | `10 == 10 && "apple" == "apple"` | Only true if **both** conditions are true. |
| `\|\|` | `10 == 10 \|\| "a" == "b"` | Always true if **one** condition is true.|

### `elif` and `else`

`if` statements only run if the condition is **True**. What happens if its **False**? That's what the `else` command is for, it's like a net that catches anything that slipped past `if`:

```R
pet_age <- 5

if (pet_age > 10) {
  print("My pet is older than 10")
} else {
  print("My pet is 10 or younger")
}
```

> `else` also needs curly brackets!

Check what happens when you change the age from 5 to 15.

Finally, what if you wanted to check another condition **only if the first one fails**? That's what `else if` is for. It's another if statement but it only runs if the first fails.

```R
pet_age = 5

if (pet_age > 10) {
  print("My pet is older than 10")
} else if (pet_age < 5) {
  print("My pet is younger than 5")
} else {
  print("My pet is 10 or younger")
}

```

You can include as many as you'd like

```R
pet_age = 5

if (pet_age > 10) {
  print("My pet is older than 10")
} else if (pet_age < 5) {
  print("My pet is younger than 5")
} else if (pet_age < 1) {
  print("My pet is freshly born")
} else {
  print("My pet is 10 or younger")
}
```

## Repeat after me

Sometimes you need to repeat a task multiple times. Sometimes hundreds. Maybe you need to loop through 1 million pieces of data. Not fun.

R's loops offer us a way to run a section of code multiple times. There are two types: `for` loops, which run the code once for each element in a sequence (like a list or string), and `while` loops, which run until some condition is false.

### `while` loops
These are almost the same as `if` statements, except for the fact that they run the code multiple times. Let's begin with a basic conditional

```python
number <- 5

if (number < 5) {
  paste(number, "is less than 10.")
}
```

> The `paste` function lets you print multiple things together

What if we wanted to check all the numbers between 5 and 10? We can use a while loop.

```python
number <- 5

while (number < 5) {
  print(paste(number, "is less than 10."))
  number <- number + 1
}
```

> We need to include `paste` inside `print` because we're doing it multiple times.

We've done two things

1. Replace `if` with `while`
2. Introduce `number = number + 1` to increase the number each time.

> Without step 2, we'd have an **infinite loop** -- one that never stops, because the condition would always be true!

While loops are useful for repeating code an indeterminate number of times.

### `for` loops
Realistically, you're most likely to use a **for** loop. They're inherently safer (you can't have an infinite loop) and often handier.

In R, `for` loops iterate through a sequence, like the objects in a list. This is more like other languages' `foreach`, than most's `for`.

Let's say you have a vector of different fruit

```R
list_of_fruits <- c("apple", "banana", "cherry")
```

and you want to run a section of code on `"apple"`, then `"banana"`, then `"cherry"`. Maybe you want to know which ones have the letter "a". We can start with a `for` loop

```R
list_of_fruits <- c("apple", "banana", "cherry")

for (fruit in list_of_fruits) {
  print(fruit)
}
```

This loop's job is to print out the variable `fruit`. But where is `fruit` defined? Well, the `for` loop runs `print(fruit)` for every element of `list_of_fruits`, **storing the current element in the variable `fruit`**. If we were to write it out explicitly, it would look like

```R
fruit <- list_of_fruits[0]
print(fruit)

fruit <- list_of_fruits[1]
print(fruit)

fruit <- list_of_fruits[2]
print(fruit)
```

Let's return to our goal: working out which ones have an "a". We need to put a **conditional** inside the loop:

```R
list_of_fruits <- c("apple", "banana", "cherry")

for (fruit in list_of_fruits) {
    if grepl("a", fruit) { 
      print(paste("a is in", fruit))
    }
    else {
      print(paste("a is not in", fruit))
    }
}
```

Finally, it's often convenient to loop through a list of numbers. R makes this easy with the `x:y` notation:

```R
1:10
```

contains all the integers between $1$ and $10$. To loop through each,

```R
for (i in 1:10) {
  print(i)
}
```

The advantage of this approach is that we can loop through many numbers:
```R
for (i in 1:1000) {
  print(i)
}
```

This can be useful if you need to loop through multiple objects by indexing.

### Mapping with purrr

Consider the follow situation. You have a dataset, and want to apply a function to *every* column. Or maybe some columns. What to do?

You *could* loop over them with a for loop. Alternatively, you could use the mapping functions in purrr, which simplifies the code.

What is a map? Generally, a map takes *something* and makes it *something else*. So far, that's the same a function. The difference is that a map takes lots of things and translates them all in the same way. For example, a geographical map takes life-sized locations and transforms them all in the same way to a hand-sized piece of paper.

Essentially, maps are a way of transforming a selection of variables in the same way. We'll start by brining in the `purrr` library

```R
library(purrr)
```

Let's use the same data as in the statistics session:

```R
library(dplyr)
players <- read.csv("data_sources/Players2024.csv")
players <- players %>% filter(positions != "Missing", height_cm > 100)
```

What if you want the median value from *all* columns? We can use the `map_dbl()` function to map *doubles* (long decimal numbers):

```R
map_dbl(players, median)
```

Don't worry about the warnings - they're just there because you can't take the median of a non-numeric variable. To check which ones are, we can map the logical operator `is.numeric`:

```R
map_lgl(players, is.numeric)
```

We can use the pipe here,

```R
players %>% map_lgl(is.numeric)
```

Let's select the numeric columns and look at the medians again

```R
players %>% 
  select_if(is.numeric) %>%
  map_dbl(median)
```

We can also create custom functions. We use `.x` to refer to the variable:

```R
players %>% 
  select_if(is.numeric) %>%
  map_dbl(~max(.x) - min(.x))
```

## Building your own machines

We'll wrap this session up by looking at custom functions. So far, we've only used built-in functions or those from other people's modules. But we can make our own!

We've only ever **called** functions - this is what we do when we use them. All functions need a **definition**, this is the code that gets run when they're called.

### The function definition

Functions are machines. They take some inputs, run some code with those inputs, and spit out **one** output. We need to define how they work before we use them. We should specify

* A name
* Some inputs
* The code to run (the machine itself)

We include these in three steps

1. The first line of the function definition (the *function signature*) specifies the name and inputs
2. We then **indent** all the code we want to run with our inputs
3. We end with a `return` statement, specifying the output


```R
insert_function_name_here <- function(input_1_name, input_2_name, ...) {
  # Code code code
}
```

For example, let's create a function that converts centimetres to metres.

```R
cm_to_m <- function(value_in_cm) {
    value_in_cm / 100
}
```

Taking it apart, we have

* **Name**: `cm_to_m`
* **Inputs** (just one): `value_in_cm`
* **Code** (just one line): `value_in_cm / 100`

Importantly, **nothing appears when you run this code**. Why? Because you've only defined the function, *you haven't used it yet*.

To use this function, we need to call it. Let's convert $10\text{ cm}$ to $\text{m}$.

```R
cm_to_m <- function(value_in_cm) {
    value_in_cm / 100
}

cm_to_m(10)
```

When we call the function, it runs with `value_in_cm <- 10`.

That's it! Every function that you use, built-in or imported, looks like this.

Because functions must be defined before called, and defining them produces no output, **best practice is to place functions at the top of your script**, below the import statements.

#### Return values and default values

One quirk of R functions is that, by default, they return the output of the line. Let's add a new line that prints the message "$x\text{ cm} = y\text{ m}$". We'll need to also save our calculation in the process:

```R
cm_to_m <- function(value_in_cm) {
    value_in_m <- value_in_cm / 100
    print(paste(value_in_cm, "cm =", value_in_m, "m"))
}

cm_to_m(10)
```

It works, but we have a problem. The output of the function is the **whole message**, not the value. The easiest way to fix this is to call the output on the last line:

```R
cm_to_m <- function(value_in_cm) {
    value_in_m <- value_in_cm / 100
    print(paste(value_in_cm, "cm =", value_in_m, "m"))
    value_in_m
}

cm_to_m(10)
```

Alternatively, you can use the `return()` function to exit before the end and manually specify the output.