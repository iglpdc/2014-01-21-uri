
# Learning the apply family of functions

One of the greatest joys of vectorized operations is being able to use the entire family of `apply` functions that are available in base `R`.

These include:

```coffee
apply
by
lapply
tapply
sapply
```

## apply

```
m <- matrix(c(1:10, 11:20), nrow = 10, ncol = 2)
# 1 is the row index
# 2 is the column index
apply(m, 1, sum)
apply(m, 2, sum)
apply(m, 1, mean)
apply(m, 2, mean)
```

## by

```coffee
head(iris)
by(iris[, 1:2], iris[,"Species"], summary)
by(iris[, 1:2], iris[,"Species"], sum)
```


## tapply
Two examples

```coffee
df <- data.frame(names = sample(c("A","B","C"), 10, rep = T), length = rnorm(10))
tapply(df$length, df$names, mean)

# Now with a more familiar dataset
tapply(iris$Petal.Length, iris$Species, mean)
```

## lapply (and llply)

What it does: Returns a list of same length as the input. 
Each element of the output is a result of applying a function to the corresponding element.

```coffee
my_list <- list(a = 1:10, b = 2:20)
lapply(my_list, mean)
```



## sapply

sapply is a more user friendly version of `lapply` and will return a list of matrix where appropriate.


Let's work with the same list we just created.

```coffee
my_list
x <- sapply(my_list, mean)
class(x)
```

## replicate

An extremely useful function to generate datasets for simulation purposes. 

```coffee
replicate(10, rnorm(10))
replicate(10, rnorm(10), simplify = TRUE)
# The final arguments turns the result into a vector or matrix if possible.
```

## mapply
Its more or less a multivariate version of `sapply`. It applies a function to all corresponding elements of each argument. 

example:

```coffee
list_1 <- list(a = c(1:10), b = c(11:20))
list_2 <- list(c = c(21:30), d = c(31:40))
mapply(sum, list_1$a, list_1$b, list_2$c, list_2$d)
```



