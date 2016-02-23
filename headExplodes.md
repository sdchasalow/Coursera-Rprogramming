### A wee example of lazy evaluation and scoping rules for argument defaults.

```R
headXplodes <- function( y, leny = n, z = m, breakme = FALSE, ...) {  
   n <- length(y)  
   if (breakme)  
      list( leny = leny, z = z )  
   else  
      list( leny = leny )  
}  
```
 
##### Default value for leny computed in body of function:

```R
headXplodes(1:10)
# $leny
# [1] 10
```

##### Default value for leny computed in body of function; ignores object n in .GlobalEnv, since looks for default value of leny first in function body.

```R
n <- 3
headXplodes(1:10)
# $leny
# [1] 10
```

##### Here, do **not** use default value.  In this case, MUST find n in the search list.  The expression (name) n is evaluated as part of the function-call expression.  If n were not found, you would never even start evaluation of the function body.

```R
headXplodes(1:10, leny = n)
# $leny
# [1] 3
```

##### Here's what happens if the default expression for an argument (in this case, the default for z) is needed but not found.  Note that this was not a problem until we set breakme = TRUE; only then did we try to access a value for z.

```R
headXplodes(1:10, breakme = TRUE)
# Error in headXplodes(1:10, breakme = TRUE) : object 'm' not found
```

##### Here, we see that the evaluator WILL look in the search list (in this case, in .GlobalEnv) for a default value if it does not find it in the function body (i.e., function environment).

```R
m <- 7
headXplodes(1:10, breakme = TRUE)
# $leny
# [1] 10
# 
# $z
# [1] 7
```

##### I have some glue in my drawer, if it helps.
