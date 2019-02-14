# Instructions for Creating Website Files

*Overview of RMD Websites and Bookdown files.* 

The RMD files go in TEXTBOOK folder.

To render all RMD files at once for a fresh website build:

```r
setwd( ...your directory here... )
rmarkdown::render_site()
```

-----

In the _site.yml file we can specify where the rendered HTML files are stored. Send them to the 'docs' folder to keep things clean. The website will live in that directory.

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

