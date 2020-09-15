This is a minimal example of a book based on R Markdown and **bookdown** (https://github.com/rstudio/bookdown). Please see the page "Get Started" at https://bookdown.org/home/about/ for how to compile this example.

I have adapted the bookdown example so that it can be used as a template to build a PDF thesis in the University of Otago styling. For this we need to grab the existing LaTeX template that the Department of Computer Science had created.

I'm uncertain if this works on Windows.

## Getting started

In R install the needed libraries plus tinytex for the PDF compilation
```
install.packages("bookdown")
install.packages("tinytex")
tinytex::install_tinytex()
```

### Open RStudio and create a new project

`File -> New Project -> New Directory -> "Book Project using bookdown"`

Give it a name and open it up.

### Grab the Otago LaTeX template

The Department of Computer Science has a LaTeX template the is available to download from https://altitude.otago.ac.nz/cs-department/otago-thesis-latex-template

we can grab it from within R using

```
download.file(url = "https://altitude.otago.ac.nz/cs-department/otago-thesis-latex-template/-/archive/master/otago-thesis-latex-template-master.zip", destfile = "otago_template.zip")
```

And then unzip it. 

```
unzip("otago_template.zip")
```

Afterwards I have a directory called `otago-thesis-latex-template-master/`.

Now we need to copy some files so that they are available to LaTeX for the building of the PDF.

The actual files we need are:

- `otagothesis.sty` which is the actual template and,
- `otago.bst` which is a citation style file.

```
file.copy("otago-thesis-latex-template-master/otagothesis.sty", "otagothesis.sty")
file.copy("otago-thesis-latex-template-master/abstract.tex", "abstract.tex")
```

We now need to change the `documentclass` in the YAML header of `index.Rmd` to be "report" instead of "book".

In `_output.yml`, on the line following `in_header: preamble.tex` we want to add an extra line with the same indentation with `before_body: frontstuff.tex`.

`frontstuff.tex` is a file containing the following in order to fill in the author declaration details from this specific template and bring in the abstract, list of tables and figures.

```
\fullname{Murray Cadzow}
\department{Department of Biochemistry}
\dob{DOB}
\address{710 Cumberland St, Dunedin, NZ}

\frontstuff
```

I also recommend adding `new_session: true` into `_bookdown.yml`. It means that each chapter gets compiled in a new R session. It can be a pain if you want to refer to objects that were created in previous chapters but it does mean that only within a chapter do your R code chunk names need to be unique, rather than across the entire book.