# Rmarkdown Website Basics

The RMD files go in TEXTBOOK.

In the _site.yml file we can specify where the rendered HTML files are stored. Send them to the 'docs' folder to keep things clean. The website will live in that directory.

Note that GitHub pages can render an MD or an RMD file, but they will not render the R code.

To include R code convert the file to an HTML, and link to that.

To link to files in other folders, you can go up a level using /../.

So for example to link to a lecture file in LECTURES from here:

```
[Lecture 01](../../LECTURES/lecture-01.html)
```

