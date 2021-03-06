# Very basics of R

R is a versatile, open source programming/scripting language that's useful both for statistics but also data science. Inspired by `S`.

* Public domain software 
* Superior (if not just comparable) to commercial alternatives
* Available on all platforms
* Not just for statistics, but also general purpose programming
* Is object oriented and functional
* Large and growing community of peers

**How to run `R`**  

You can run `R` interactively or in batch mode.

*Interactive mode*  
e.g. type in `R` from the shell. The window that appears is called the R console. Any command you type into the prompt is interpreted by the R kernel. An output may or may not be printed to the screen depending on the types of commands that you run.

*Batch mode*  
You can also run one or more R scripts in batch mode.

```
R CMD BATCH script_1.R script_2.R
```
You can also script inline using `Rscript -e`. *Example*

```coffee
Rscript -e "library(knitr); knit('script.Rmd')"
# Notice how I used a semi-colon to separate multiple commands in a single line
```

**Viewing objects in your global environment and how to clean them up**

List objects in your current environment

```
ls()
```

remove objects from your current environment

```
x <- 5
rm(x)
x
```

remove all objects from your current environment

```
rm(list = ls())
```

Use `#` signs to comment. Comment liberally in your R scripts. Anything to the right of a `#` is ignored. 

**Assignment operator**

`<- ` is the assignment operator. Assigns values on the right to objects on the left. Mostly similar to `=` but not always. Learn to use `<-` as it is good programming practice. Using `=` in place of `<-` can lead to issues down the line.

**Package management**

Use `old.packages()` to keep track of what's out of date.  
`update.packages()` - with package name will update a single package. Otherwise it will update all interactively. This can take a while if you haven't done it recently. To update everything without any user intervention, use the `ask = F` argument.

```
update.packages(ask = FALSE)
```


**Quitting R**

type in `quit()` or `q()` and answer `Y` to quit.
