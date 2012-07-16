
## Basic markdown functionality

# Heading 1
## Heading 2
### Heading 3
#### Heading 4




### Basic formatting
Bold text: **Bold text**
Italics text: *italics text*




### Dot Points
Simple dot points:

* Point 1
* Point 2
* Point 3

and numeric dot points:

1. Number 1
2. Number 2
3. Number 3

and nested dot points:

* A
    * A.1
    * A.2
* B
    * B.1
    * B.2


### Equations
Uses Mathjax to support LaTeX equations.

Inline equations: e.g., $y_i = \alpha + \beta x_i + e_i$.

Displayed equations:

$$
\frac{1}{1+\exp(-x)}
$$





### Hyperlinks

* [my RSS feed](http://feeds.feedburner.com/jeromyanglim).
* <http://www.r-project.org/>

### Images

![image from redmond barry building unimelb](http://i.imgur.com/RVNmr.jpg)



### Code
Inline code between backticks: e.g., `print('hello world!')`.

Displayed code can be tab indented or four space indented:

    ```{r}
    x <- 1:10
    x
    ```
    



And then there's inline code such as `x <- 1:10`.

### Quote
Quotes by adding greater than to start of each line.

> To be, or not to be, that is the question:
> Whether 'tis nobler in the mind to suffer
> The slings and arrows of outrageous fortune,


### Tables
Extended github table functionality:

A   | B      |   C
--- | ---    | ---
1   | Male   | Blue
2   | Female | Pink

Or just write HTML:

<table border="1">
    <tr>
		<td>Cell A1</td>
        <td>Cell B1</td>
    </tr>
    <tr>
        <td>Cell A2</td>
        <td>Cell B2</td>
	</tr>
</table>






## Prepare for analyses


```r
set.seed(1234)
library(ggplot2)
library(lattice)
```





## Basic console output
To insert an R code chunk, you can type it manually or just press `Chunks - Insert chunks` or use the shortcut key. This will produce the following code chunk:

    ```{r}
    
    ```

Pressing tab when inside the braces will bring up code chunk options.

The following R code chunk labelled `basicconsole` is as follows:

    ```{r basicconsole}
    x <- 1:10
    y <- round(rnorm(10, x, 1), 2)
    df <- data.frame(x, y)
    df
    ```
The code chunk input and output is then displayed as follows:



```r
x <- 1:10
y <- round(rnorm(10, x, 1), 2)
df <- data.frame(x, y)
df
```



```
##     x    y
## 1   1 1.31
## 2   2 2.31
## 3   3 3.36
## 4   4 3.27
## 5   5 5.04
## 6   6 6.11
## 7   7 8.43
## 8   8 8.98
## 9   9 8.38
## 10 10 9.27
```




## Plots
Images generated by `knitr` are saved in a figures folder. However, they also appear to be represented in the HTML output using a [data URI scheme]( http://en.wikipedia.org/wiki/Data_URI_scheme). This means that you can paste the HTML into a blog post or discussion forum and you don't have to worry about finding a place to store the images; they're embedded in the HTML.

### Simple plot
Here is a basic plot using base graphics:

    ```{r simpleplot}
    plot(x)
    ```



```r
plot(x)
```

![plot of chunk simpleplot](figure/simpleplot.png) 


Note that unlike traditional Sweave, there is no need to write `fig=TRUE`.


### Multiple plots
Also, unlike traditional Sweave, you can include multiple plots in one code chunk:

    ```{r multipleplots}
    boxplot(1:10~rep(1:2,5))
    plot(x, y)
    ```



```r
boxplot(1:10 ~ rep(1:2, 5))
```

![plot of chunk multipleplots](figure/multipleplots1.png) 

```r
plot(x, y)
```

![plot of chunk multipleplots](figure/multipleplots2.png) 


### `ggplot2` plot
Ggplot2 plots work well:



```r
qplot(x, y, data = df)
```

![plot of chunk ggplot2ex](figure/ggplot2ex.png) 


### `lattice` plot
As do lattice plots:



```r
xyplot(y ~ x)
```

![plot of chunk latticeex](figure/latticeex.png) 


Note that unlike traditional Sweave, there is no need to print lattice plots directly.

## R Code chunk features
### Create Markdown code from R
The following code hides the command input (i.e., `echo=FALSE`), and outputs the content directly as code (i.e., `results=asis`, which is similar to `results=tex` in Sweave).


    ```{r dotpointprint, results='asis', echo=FALSE}
    cat("Here are some dot points\n\n")
    cat(paste("* The value of y[", 1:3, "] is ", y[1:3], sep="", collapse="\n"))
    ```

Here are some dot points

* The value of y[1] is 1.31
* The value of y[2] is 2.31
* The value of y[3] is 3.36


### Create Markdown table code from R
    ```{r createtable, results='asis', echo=FALSE}
    cat("x | y", "--- | ---", sep="\n")
    cat(apply(df, 1, function(X) paste(X, collapse=" | ")), sep = "\n")
    ```

x | y
--- | ---
1 | 1.31
2 | 2.31
3 | 3.36
4 | 3.27
5 | 5.04
6 | 6.11
7 | 8.43
8 | 8.98
9 | 8.38
10 | 9.27



### Control output display
The folllowing code supresses display of R input commands (i.e., `echo=FALSE`)
and removes any preceding text from console output (`comment=""`; the default is `comment="##"`).

    ```{r echo=FALSE, comment="", echo=FALSE}
    head(df)
    ```



```
  x    y
1 1 1.31
2 2 2.31
3 3 3.36
4 4 3.27
5 5 5.04
6 6 6.11
```






### Control figure size
The following is an example of a smaller figure using `fig.width` and `fig.height` options.

    ```{r smallplot, fig.width=3, fig.height=3}
    plot(x)
    ```



```r
plot(x)
```

![plot of chunk smallplot](figure/smallplot.png) 


### Cache analysis
Caching analyses is straightforward.
Here's example code. 
On the first run on my computer, this took about 10 seconds.
On subsequent runs, this code was not run. 

If you want to rerun cached code chunks, just [delete the contents of the `cache` folder](http://stackoverflow.com/a/10629121/180892)

    ```{r longanalysis, cache=TRUE}
    for (i in 1:5000) {
        lm((i+1)~i)
    }
    ```



## Conclusion

* R Markdown is awesome. 
    * The ratio of markup to content is excellent. 
    * For exploratory analyses, blog posts, and the like R Markdown will be a powerful productivity booster. 
    * For journal articles, LaTeX will presumably still be required.
* The RStudio team have made the whole process very user friendly.
    * RStudio provides useful shortcut keys for compiling to HTML, and running code chunks. These shortcut keys are presented in a clear way.
    * The incorporated extensions to Markdown, particularly formula and table support, are particularly useful.
    * Jump-to-chunk feature facilitates navigation. It helps if your code chunks have informative names.
    * Code completion on R code chunk options is really helpful. See also [chunk options documentation on the knitr website](http://yihui.name/knitr/options).
* Other recent posts on R markdown include those by :
     * [Christopher Gandrud](http://christophergandrud.blogspot.com.au/2012/05/dynamic-content-with-rstudio-markdown.html)
     * [Markcus Gesmann](http://lamages.blogspot.com.au/2012/05/interactive-reports-in-r-with-knitr-and.html)
     * [Rstudio on R Markdown](http://rstudio.org/docs/authoring/using_markdown)
     * [Yihui Xie](http://yihui.name/knitr/): I really want to thank him for developing `knitr`. 
     He has also posted [this example of R Markdown](https://github.com/yihui/knitr/blob/master/inst/examples/knitr-minimal.Rmd).


## Questions
The following are a few questions I encountered along the way that might interest others.
### Annoying `<br/>`'s
**Question:** I asked on the Rstudio discussion site:
[Why does Markdown to HTML insert `<br/>` on new lines?](http://support.rstudio.org/help/discussions/problems/2329-why-does-r-markdown-to-html-insert-br-when-there-is-a-new-line-of-text)

**Answer:** I just do a find and delete on this text for now.

### Temporarily disable caching
**Question:** I asked on StackOverflow about 
[How to set cache=FALSE for a knitr markdown document and override code chunk settings?](http://stackoverflow.com/q/10628665/180892)

**Answer:**  Delete the cache folder. But there are other possible workflows.

### Equivalent of Sexpr
**Question:** I asked on Stack Overvlow about [whether there an R Markdown equivalent to Sexpr in Sweave?](http://stackoverflow.com/q/10629416/180892).

**Answer:** Include the code between brackets of "backick r space" and "backtick". 
E.g., in the source code I have calculated 2 + 2 = `4` .