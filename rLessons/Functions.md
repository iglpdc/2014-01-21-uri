Functions, Control Structures and Documentation
========================================================

*Note: This lesson is mostly from Karthik Ram's [material from the Canberra Software Carpentry R Bootcamp](https://github.com/swcarpentry/2013-10-09-canberra).  Anything good is there becuase of Karthik.  Mistakes are all mine.*

# Lesson Index:
- [Functions](#functions)
- [Control Structures](#control-strucutres)
- [Documentation](#documentation)
- [Exercise](#function-exercise)

# Functions 

Functions represent one of the most powerful features of the R language. By creating functions, you can go past just using what others have written and create your own.

After a while operations at the interactive prompt are not enough. You have special operations that need to be grouped together. That's when you'll need to write functions.

* Basic of how to write functions
* Lexical scoping and scoping rules
* Example

Functions are created using the `function()` directive.
They are stored as any other R object.  They are of class "function"

**Basic syntax**

```
func <- function(<take some input>)  {
    # run a whole bunch of your code
}
```

Functions are first class objects. Treat them the same way as any other objects. This means you can pass them to other functions (as you would do with say vectors). Very useful feature for statistical analyses. We can also nest functions inside each other.

Return value is the very last R expression to be evaluated. No special expression for returning although you can use return()

Functions have named arguments and these can have default values. Arguments used inside a function definition are called formal arguments.

use `formals()` to see what they are.

Not every function makes use of all formal arguments.

Function arguments can be missing or might just use default values. Formal arguments are included in the function definition.

If a function has 10 arguments, you don't have to specify all of them. 

R function arguments can be matched positionally or by name.


```r
args(sd)
```

Takes x, a vector of data. 
Default is `na.rm` is false.
So you don't have to do anything.


```r
dat <- rnorm(1000)
sd(dat)  # Matched positionally
sd(x = dat)  # matched by name
sd(x = dat, na.rm = FALSE)
# When naming you can switch them but not recommended
sd(na.rm = FALSE, dat)
```


Even though it's allowed, don't switch positions on names.

See 


```r
args(lm)
```


It's good to name arguments when you have a long list to work through. Arguments can also be partially matched.


```r
add <- function(x = 1, y = 2) {
    x + y
}
```


R does what's known as lazy evaluation. Arguments to functions are lazily evaluated. Common model in many languages. Only evaluated as they are needed.


```r
add <- function(a, b) {
    a^2
}
```


The function call never uses b. So calling `f(10)` will not produce an error because `a` got positionally matched.

Another example of lazy evaluation



```r
add <- function(a, b) {
    print(a^2)
    print(b^2)
}
```


`add(10)` will work until it reaches a point where the function will break.

**The three dot operator.**

`...` are used when extending another function and you don't want to copy the entire list of arguments from the original function.

Also useful when the number of arguments cannot be known in advance. For example `paste()` takes a variable number of arguments. Function does not know how many things it's going to paste together.


## Writing functions in R

If you have to repeat the same few lines of code more than once, then you really need to write a function. Functions are a fundamental building block of R. You use them all the time in R and it's not that much harder to string functions together (or write entirely new ones from scratch) to do more.

* R functions are objects just like anything else. 
* By default, R function arguments are lazy - they're only evaluated if they're actually used:
* Every call on a R object is almost always a function call.

### Basic components of a function

* The `body()`, the code inside the function.
* The `formals()`, the "formal" argument list, which controls how you can call the function.
* The `environment()`` which determines how variables referred to inside the function are found.
* `args()` to list arguments.


```r
f <- function(x) x
f

formals(f)

environment(f)
```



## More on environments
Variables defined inside functions exist in a different environment than the global environment. However, if a function is not defined inside one, it will look one level above.

For example.

```
x <- 2
g <- function() { 
  y <- 1
  c(x, y)
}  
g()
rm(x, g)
```

Same rule applies for nested functions.

A first useful function.


```r
first <- function(x, y) {
    z <- x + y
    return(z)
}
```



```r
add <- function(a, b) {
    return(a + b)
}

add(c(1, 2, 3, 4), 1)
```


## What does this function return?


```r
x <- 5
f <- function() {
    y <- 10
    c(x = x, y = y)
}
f()
```


## What does this function return?


```r
x <- 5
g <- function() {
    x <- 20
    y <- 10
    c(x = x, y = y)
}
g()
```


## What does this function return??


```r
x <- 5
h <- function() {
    y <- 10
    i <- function() {
        z <- 20
        c(x = x, y = y, z = z)
    }
    i()
}
h()
```



## Functions with pre defined values


```r
temp <- function(a = 1, b = 2) {
    return(a + b)
}
```


## Functions usually return the last value it computed

```
f <- function(x) {
  if (x < 10) {
    0
  } else {
    10
  }
}
f(5)
f(15)
```

# Control Structures

These allow you to control the flow of execution of a script typically inside of a function.
Common ones include:

* if, else
* for
* while
* repeat
* break
* next
* return

We don't use these while working with R interactively but rather inside functions. 

## If/else


```r
if(condition) {
    # do something 
} else { 
    # do something else
    }
}
```

e.g. 


```r
x <- 1:15
if (sample(x, 1) <= 10) {
    print("x is less than 10")
} else {
    print("x is greater than 10")
}
```


**Shorthand for ifelse**

`ifelse(sample(x, 1) <10, "x less than 10", "x greater than 10")`

Other valid ways of writing if/else


```r
if (sample(x, 1) < 10) {
    y <- 5
} else {
    y <- 0
}
```



```r
y <- if (sample(x, 1) < 10) {
    5
} else {
    0
}
```


## for

For loops work on an iterable variable and assign successive values till the end of a sequence.


```r
for (i in 1:10) {
    print(i)
}
```



```r
x <- c("apples", "oranges", "bananas", "strawberries")

for (i in x) {
    print(x[i])
}

for (i in 1:4) {
    print(x[i])
}

for (i in seq(x)) {
    print(x[i])
}

for (i in 1:4) print(x[i])
```


Nested loops


```r
m <- matrix(1:10, 2)
for (i in seq(nrow(m))) {
    for (j in seq(ncol(m))) {
        print(m[i, j])
    }
}
```


## While

```
i <- 1
while(i < 10) {
    print(i)
    i <- i + 1
}
```
Be sure there is a way to exit out of a while loop.

## Repeat and break


```r
repeat {
    # simulations; generate some value have an expectation if within some range,
    # then exit the loop
    if ((value - expectation) <= threshold) {
        break
    }
}
```


## Next


```r
for (i in 1:20) {
    if (i%%2 == 1) {
        next
    } else {
        print(i)
    }
}
```


This loop will only print even numbers and skip over odd numbers. In the afternoon we'll learn other functions that will help us avoid these types of slow control flows as much as possible (mostly the while and for loops).

# Documentation

Anyone who has worked with computers at any level for any period of time will attest to the fact that keeping track of your workflow and understanding what was done and why gets problematic quickly.  Tomorrow we will introduce a solution of this that deals with managing multiple versions (i.e. git).  

But, at an even more basic level than versioning, is the issue of being able to read a piece of code and understand what it is doing.  The primary solution for creating understandable code is documentation and you already have been doing some of this if you have been using comment tags (`#`).  

These are simple to implement, can be placed anywhere in your code.  

For instance:


```r
# Here is an example of a simple comment This function, takes and argment
# and prints it to the screen
x <- function(arg) {
    print(arg)  # Printing the arg
}

x("Documentation is cool!")

```


These simple comments are likely enough just for scripts, but as you begin to use functions it better to follow a more standard format.  One such format that is widely used in package development is [Roxygen](http://roxygen.org), a literate programming .  Its current implementation in R is with `roxygen2`.  It allows for simple documenting of code and easily converting that code into manual pages.  We won't go that far, but learning to document the basics with Roxygen style comments (`#'`) and tags (`@`) will put you much closer to being able to develop packages.

Roxygen Comment and Tags


```r
#' This is an Roxygen comment!  Pretty difficult, eh?
#' @param This is an Roxygen tag
```


So to use these to document a function would look like:


```r
#' My Simple Function
#'
#' This function takes an argument and prints it to the screen
#' 
#' @param args This is an input argument.  May be of any type
#' 
#' @return returns an object of the same type as the input.
#'
#' @export 
#'
#' @examples
#' x('Roxygen style documentation is cooler')
#' x('#''>'#')

x <- function(arg) {
    # You can still use regular comments, if you like.  This is probably a good
    # idea, too.
    return(arg)
}

x("Roxygen style documentation is cooler")
x("#'" > "#")
```


** note: `@export` and `@examples` are important for package development.  I only include them here to let you know there are many tags you can use **

So, while it takes a bit more time to document as you write, it will pay dividends in the future as your properly documented code will be

- Easier for you to understand when you come back to it later
- Easier for others to understand
- _MUCH_ easier to convert a collection of functions into a package

To find out more, Hadley Wickham's [Advanced R Programming](http://adv-r.had.co.nz/Documenting-functions.html) is a great resource.

# Function Exercise
Continuing with our NLA example, this exercise will have you create several functions that work on that dataset.

1. Create a function (with proper documentation, but no need to inclue `@export` or `@examples`) that takes a vector as input and returns it's mean.

2. Create another function (again with good documentation), that allows us to deal with `NA` values (hint: think `mean()' and `...`).

3. Create another function (do I have to say it?) that allows you to calculate the mean and standard deviation (hint: `sd()`) of the input and still deals with `NA`.  You will probably want to have a third argument in your function and will need to use a control structure from above.

4. Now use this function you have created to get mean and standard deviation of  two of the data columns in your NLA data frame you created earlier.  If you no longer have that in memory, you can use `read.csv()` and the .csv file you wrote to disk.

