webshot
=======

**Webshot** makes it easy to take screenshots of web pages from R. It can also run Shiny applications locally and take screenshots of the app.


## Installation

It requires an installation of the external program [PhantomJS](http://phantomjs.org/). Please make sure you have PhantomJS version 2 or higher installed. Previous versions may have trouble rendering some fonts.

Once PhantomJS is installed you can install webshot with:

```R
devtools::install_github("wch/webshot")
```


## Usage

By default, `webshot` will use a 992x744 pixel viewport (a virtual browser window) and take a screenshot of the entire page, even the portion outside the viewport.

```R
library(webshot)
webshot("http://www.rstudio.com/", "rstudio.png")
webshot("http://www.rstudio.com/", "rstudio.pdf") # Can also output to PDF
```

You can clip it to just the viewport region:

```R
webshot("http://www.rstudio.com/", "rstudio-viewport.png", cliprect = "viewport")
```

You can also get screenshots of a portion of a web page using CSS selectors. If there are multiple matches for the CSS selector, it will use the first match.

```R
webshot("http://www.rstudio.com/", "rstudio-header.png", selector = "#header")
```

If you supply multiple CSS selectors, it will take a screenshot containing all of the selected items.

```R
webshot("http://rstudio.com/", "rstudio-selectors.png",
        selector = c(".header-v1", "#content-boxes-1")))
```

The clipping rectangle can be expanded to capture some area outside the selected items:

```R
webshot("http://rstudio.com/", "rstudio-boxes.png",
        selector = "#content-boxes-1",
        expand = c(40, 10, 0, 10))
```


### Screenshots of Shiny applications

The `appshot()` function will run a Shiny app locally in a separate R process, and take a screenshot of it. After taking the screenshot, it will kill the R process that is running the Shiny app.

```R
# Get the directory of one of the Shiny examples
appdir <- system.file("examples", "01_hello", package="shiny")
appshot(appdir, "01_hello.png")
```

### Manipulating images

If you have GraphicsMagick (recommended) or ImageMagick installed, you can pass the result to `resize()` to resize the image after taking the screenshot. This can take any valid ImageMagick geometry specifictaion, like `"75%"`, or `"400x"` (for an image 400 pixels wide).

You can also call `shrink()` function, which runs [OptiPNG](http://optipng.sourceforge.net/) to shrink the PNG file losslessly.

```R
webshot("http://www.google.com/", "google-small.png") %>%
 resize("75%") %>%
 shrink()

webshot("http://www.google.com/", "google-small.png") %>%
 resize("400x") %>%
 shrink()
```
