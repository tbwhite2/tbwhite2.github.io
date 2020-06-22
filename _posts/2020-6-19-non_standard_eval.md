
---
layout: post
title: What the HECK is NSE
---


Non standard evaluation (NSE) sounds like a scary subject. In this post, I'll work to demystify the 
topic by comparing it to standard evaluation (SE) and showing real world examples of NSE. 

### What the HECK is NSE
NSE is the the method by which several great features in R work that you probably use every day without
realizing you are using NSE.  The easiest way to explain NSE is to compare it to SE, R's default behavior.
In R, when you assign a value to a variable and call that variable, you get the value back. We can 
see this as:


```r
x = 'cats'
x
```

```
## [1] "cats"
```
In contrast when you call the same variable 'x' in NSE using the quote() function, you get back 'x'.

```r
quote(x)
```

```
## x
```
Instead of seeing 'x' and returning what x is referencing ('cat'), quote sees the code used to compute
the value.  NSE doesn't check if a variable has defined in its environment and it doesn't execute
code that it evaluates.  It simply sees the argument as a string and returns it:

```r
quote(y)
```

```
## y
```

```r
quote(1+3)
```

```
## 1 + 3
```
### The functions of NSE
NSE uses four main functions: quote(), substitute(), enquote(), deparse(), eval()/get()

quote, substitute, and enquote are the workhorses of NSE.  They operate slightly differently. quote() 
returns the literal string of the argument passed to it.  substitute() does the same, but will search 
up an environment chain for the original reference.  This idea is more easily explained in an example 
such as below:


```r
external_ref = 'GO VOLS'
nse_example = function(internal_ref){
  list(quote = quote(internal_ref),
       substitute = substitute(internal_ref),
       SE = internal_ref)
}
nse_example(external_ref)
```

```
## $quote
## internal_ref
## 
## $substitute
## external_ref
## 
## $SE
## [1] "GO VOLS"
```

As the above example shows, unlike quote which returns exactly what was passed to it, substitute looked
up the environment chain to the environment outside the function and found the reference 'external_ref'.
This seems black magicy to me, but it works via a concept called a promise.  I won't cover a promise
here but can point the reader in the right direction here[].

enquote is a shorthand function for quote(some_function(....)) which just means it looks inside the 
function and performs the quote operation on its contents, such as below

```r
test_function = function(x,y){
  x*2 + y
}
enquote(test_function)
```

```
## base::quote(function(x,y){
##   x*2 + y
## })
```

```r
quote(test_function)
```

```
## test_function
```

deparse simply converts the output of a NSE function call into a character vector.

```r
x = 'GBO'
thing = substitute(x)
deparse(thing)
```

```
## [1] "x"
```

eval enables the user to run the product of substitute and get allows the user to run or return the values of a string object.  Combining them with a NSE output gives SE behavior


```r
NSE_obj = quote(2+4)
eval(NSE_obj)
```

```
## [1] 6
```

```r
best_mascot = 'smokey'
NSE_obj = deparse(quote(best_mascot))
get(NSE_obj)
```

```
## [1] "smokey"
```

### Where is NSE used

We've actually used NSE in an unexpected way already in this post.  When we constructed the list object
in nse_example() we were using NSE to not have to enquote the names of the list elements.  By using a
combination of substitute() to capture the argument and eval() to deploy it.  Other common examples 
are not quoting library names when attaching a package by calling library() or by column names being 
inherited from their objects in data.frame.


```r
library(magrittr)
library('magrittr')
```


```r
a = 1:5
b = 6:10
data.frame(a,b)
```

```
##   a  b
## 1 1  6
## 2 2  7
## 3 3  8
## 4 4  9
## 5 5 10
```



### OK that's great - Why do I care?

For 99% of developers I'd argue you shouldn't really care about using NSE in practice.  When using 
NSE, you can create very powerful and elegant functions.  However, code using NSE is harder to read
and share.  Additionally, functions lose a property called referential transparency when invoking NSE.
Referential transparency basically means you can replace arguments with their values and function
behavior doesn’t change.  

I think its important to understand what NSE is so you know what it means when people talk about NSE
and so you have some background for how some functions are doing the things they do.  I don't see myself 
using much NSE in my day to day code as in most cases the same task can be completed with more clear code.  

As often as possible it makes sense to code to be understood rather than clever :).
