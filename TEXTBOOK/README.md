# Instructions for Creating Website Files

*Overview of [RMD Websites](https://bookdown.org/yihui/rmarkdown/rmarkdown-site.html) opitions in Bookdown.* 

The RMD files go in TEXTBOOK folder. To render all RMD files at once for a fresh website build:

```r
# open RMD file from the website directory or setwd()
# then run this in the R console in R Studio
# render the entire site
rmarkdown::render_site()

# render a single file only
rmarkdown::render_site("about.Rmd")
```

For syntax highlighting styles check out this [GALLERY](http://animation.r-forge.r-project.org/knitr/) of options.

For theme examples try [HERE](http://www.datadreaming.org/post/r-markdown-theme-gallery/).

Or to generate all of the **knitr** package theme styles for preview try:

```r
library(knitr)
themes = knit_theme$get()
pat_brew()  # use brew patterns <%  %>
for (theme in themes) knit('theme.brew', paste('theme-', theme, '.Rhtml', sep = ''))
knit_patterns$restore() # clear patterns

mapply(knit, input = list.files(pattern = '\\.Rhtml$'))

writeLines(c(
  '<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en-us" lang="en-us">
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8" />
<title>Highlight themes in knitr</title>
<meta name="author" content="Taiyun Wei and Yihui Xie" />
</head>
<body>', 
  paste(sprintf('<iframe src="%s" width="800" height="520" scrolling="auto" style="display: block; margin: auto;"></iframe>',
                list.files(pattern = '^theme-.*\\.html$')), collapse = '<hr />\n'),
  '</body>
</html>'),
  con = 'index.html')

browseURL('index.html')
```



-----

# HTML Settings

The HTML options are set in the file called *_site.yml*. 

For example, we can specify where the rendered HTML files are stored. Send them to the 'docs' folder to keep things clean. The website is hosted from that directory.

You will also add new navbar items in the _site.yml file:

```
name: "Program Eval III"
output_dir: "../docs"
navbar:
  title: "My Website"
  left:
    - text: "Home"
      href: index.html
    - text: "About"
      href: about.html
    - text: "Data"
      href: Datasets_in_R.html
    - text: "Specification"
      href: specification-part-I.html
output:
  bookdown::html_document2:
    theme: yeti
    toc: yes
    toc_depth: 3
    toc_float: true
    number_sections: yes
    highlight: haddock
```

-----

Note that GitHub pages can render an MD or an RMD file, but they will not render the R code.

To include R code convert the file to an HTML, and link to that.

To link to files in other folders, you can go up a level using /../.

So for example to link to a lecture file in LECTURES from here:

```
[Lecture 01](../../LECTURES/lecture-01.html)
```

