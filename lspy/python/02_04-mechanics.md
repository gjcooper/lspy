(mechanics)=
# Additional Python concepts

> *Form follows function*
>
>-- Louis Sullivan

In Chapter \@ref(introPython) our main goal was to get started in Python. As we go through the book we'll run into a lot of new Python concepts, which I'll explain alongside the relevant data analysis concepts. However, there's still quite a few things that I need to talk about now, otherwise we'll run into problems when we start trying to work with data and do statistics. So that's the goal in this chapter: to build on the introductory content from the last chapter, to get you to the point that we can start using Python for statistics. Broadly speaking, the chapter comes in two parts. The first half of the chapter is devoted to the "mechanics" of Python: installing and loading packages, managing the workspace, navigating the file system, and loading and saving data. In the second half, I'll talk more about what kinds of variables exist in Python, and introduce three new kinds of variables: factors, data frames and formulas. I'll finish up by talking a little bit about the help documentation in Python as well as some other avenues for finding assistance. In general, I'm not trying to be comprehensive in this chapter, I'm trying to make sure that you've got the basic foundations needed to tackle the content that comes later in the book. However, a lot of the topics are revisited in more detail later, especially in Chapters \@ref(datahandling) and \@ref(scripting). 

(comments)=
## Using comments

Before discussing any of the more complicated stuff, I want to introduce the **_comment_** character, `#`. It has a simple meaning: it tells Python to ignore everything else you've written on this line. You won't have much need of the `#` character immediately, but it's very useful later on when writing scripts (see Chapter \@ref(scripting)). However, while you don't need to use it, I want to be able to include comments in my Python extracts. For instance, if you read this:[^keeper]
```{r}
seeker <- 3.1415           # create the first variable
lover <- 2.7183            # create the second variable
keeper <- seeker * lover   # now multiply them to create a third one
print( keeper )            # print out the value of 'keeper'

```
it's a lot easier to understand what I'm doing than if I just write this:
```{r}
seeker <- 3.1415
lover <- 2.7183
keeper <- seeker * lover
print( keeper )    
```
You might have already noticed that the code extracts in Chapter \@ref(introPython) included the `#` character, but from now on, you'll start seeing `#` characters appearing in the extracts, with some human-readable explanatory remarks next to them. These are still perfectly legitimate commands, since Python knows that it should ignore the `#` character and everything after it. But hopefully they'll help make things a little easier to understand.

(packageinstall)=
## Installing and loading packages


In this section I discuss Python **_packages_**, since almost all of the functions you might want to use in Python come in packages. A package is basically just a big collection of functions, data sets and other Python objects that are all grouped together under a common name. Some packages are already installed when you put Python on your computer, but the vast majority of them of Python packages are out there on the internet, waiting for you to download, install and use them. 

When I first started writing this book, Python didn't really exist as a viable option for using Python, and as a consequence I wrote a very lengthy section that explained how to do package management using raw Python commands. It's not actually terribly hard to work with packages that way, but it's clunky and unpleasant. Fortunately, we don't have to do things that way anymore. In this section, I'll describe how to work with packages using the Python tools, because they're so much simpler. Along the way, you'll see that whenever you get RStudio to do something (e.g., install a package), you'll actually see the Python commands that get created. I'll explain them as we go, because I think that helps you understand what's going on. 

However, before we get started, there's a critical distinction that you need to understand, which is the difference between having a package **_installed_** on your computer, and having a package **_loaded_** in Python. As of this writing, there are just over 5000 Python packages freely available "out there" on the internet.[^n_packages] When you install Python on your computer, you don't get all of them: only about 30 or so come bundled with the basic Python installation. So right now there are about 30 packages "installed" on your computer, and another 5000 or so that are not installed. So that's what installed means: it means "it's on your computer somewhere". The critical thing to remember is that just because something is on your computer doesn't mean Python can use it. In order for Python to be able to *use* one of your 30 or so installed packages, that package must also be "loaded". Generally, when you open up Python, only a few of these packages (about 7 or 8) are actually loaded. Basically what it boils down to is this:

>A package must be installed before it can be loaded.

>A package must be loaded before it can be used.

This two step process might seem a little odd at first, but the designers of Python had very good reasons to do it this way,[^load_separate] and you get the hang of it pretty quickly.


### The package panel in RStudio


```{r packagepanel, fig.cap="The packages panel.", echo=FALSE}
knitr::include_graphics(here::here("bookdown", "img", "mechanics", "Rstudiopackages.png"))
```

Right, lets get started. The first thing you need to do is look in the lower right hand panel in RStudio. You'll see a tab labelled "Packages". Click on the tab, and you'll see a list of packages that looks something like Figure \@ref(fig:packagepanel). Every row in the panel corresponds to a different package, and every column is a useful piece of information about that package.[^library] Going from left to right, here's what each column is telling you:


- The check box on the far left column indicates whether or not the package is loaded.
- The one word of text immediately to the right of the check box is the name of the package.
- The short passage of text next to the name is a brief description of the package.
- The number next to the description tells you what version of the package you have installed.
- The little x-mark next to the version number is a button that you can push to uninstall the package from your computer (you almost never need this).


(packageload)=
### Loading a package

That seems straightforward enough, so let's try loading and unloading packades. For this example, I'll use the `foreign` package. The `foreign` package is a collection of tools that are very handy when Python needs to interact with files that are produced by other software packages (e.g., SPSS). It comes bundled with Python, so it's one of the ones that you have installed already, but it won't be one of the ones loaded. Inside the `foreign` package is a function called `read.spss()`. It's a handy little function that you can use to import an SPSS data file into Python, so let's pretend we want to use it. Currently, the `foreign` package isn't loaded, so if I ask Python to tell me if it knows about a function called `read.spss()` it tells me that there's no such thing...
```{r}
exists( "read.spss" )
```
Now let's load the package. In RStudio, the process is dead simple: go to the package tab, find the entry for the `foreign` package, and check the box on the left hand side. The moment that you do this, you'll see a command like this appear in the Python console:
```{r eval=FALSE}
library("foreign", lib.loc="/Library/Frameworks/R.framework/Versions/3.0/Resources/library")
```
The `lib.loc` bit will look slightly different on Macs versus on Windows, because that part of the command is just RStudio telling Python where to look to find the installed packages. What I've shown you above is the Mac version. On a Windows machine, you'll probably see something that looks like this:
```{r eval=FALSE}
library("foreign", lib.loc="C:/Program Files/R/R-3.0.2/library")
```
But actually it doesn't matter much. The `lib.loc` bit is almost always unnecessary. Unless you've taken to installing packages in idiosyncratic places (which is something that you can do if you really want) Python already knows where to look. So in the vast majority of cases, the command to load the `foreign` package is just this:
```{r}
library("foreign")
```
Throughout this book, you'll often see me typing in `library()` commands. You don't actually have to type them in yourself: you can use the RStudio package panel to do all your package loading for you. The only reason I include the `library()` commands sometimes is as a reminder to you to make sure that you have the relevant package loaded. Oh, and I suppose we should check to see if our attempt to load the package actually worked. Let's see if Python now knows about the existence of the `read.spss()` function...
```{r}
exists( "read.spss" )
```
Yep. All good.



(packageunload)=
### Unloading a package


Sometimes, especially after a long session of working with Python, you find yourself wanting to get rid of some of those packages that you've loaded. The RStudio package panel makes this exactly as easy as loading the package in the first place. Find the entry corresponding to the package you want to unload, and uncheck the box. When you do that for the `foreign` package, you'll see this command appear on screen:
```{r}
detach("package:foreign", unload=TRUE)
```
And the package is unloaded. We can verify this by seeing if the `read.spss()` function still `exists()`: 
```{r}
exists( "read.spss" )
```
Nope. Definitely gone. 



### A few extra comments

Sections \@ref(packageload) and \@ref(packageunload) cover the main things you need to know about loading and unloading packages. However, there's a couple of other details that I want to draw your attention to. A concrete example is the best way to illustrate. One of the other packages that you already have installed on your computer is the `Matrix` package, so let's load that one and see what happens:
```{r eval=FALSE}
library( Matrix )

## Loading required package: lattice
```
This is slightly more complex than the output that we got last time, but it's not too complicated. The `Matrix` package makes use of some of the tools in the `lattice` package, and Python has kept track of this dependency. So when you try to load the `Matrix` package, Python recognises that you're also going to need to have the `lattice` package loaded too. As a consequence, *both* packages get loaded, and Python prints out a helpful little note on screen to tell you that it's done so. 

Python is pretty aggressive about enforcing these dependencies. Suppose, for example, I try to unload the `lattice` package while the `Matrix` package is still loaded. This is easy enough to try: all I have to do is uncheck the box next to "lattice" in the packages panel. But if I try this, here's what happens:
```{r eval=FALSE}
detach("package:lattice", unload=TRUE)

## Error: package `lattice' is required by `Matrix' so will not be detached
```
Python refuses to do it. This can be quite useful, since it stops you from accidentally removing something that you still need. So, if I want to remove both `Matrix` and `lattice`, I need to do it in the correct order


Something else you should be aware of. Sometimes you'll attempt to load a package, and Python will print out a message on screen telling you that something or other has been "masked". This will be confusing to you if I don't explain it now, and it actually ties very closely to the whole reason why Python forces you to load packages separately from installing them. Here's an example. Two of the package that I'll refer to a lot in this book are called `car` and `psych`. The `car` package is short for "Companion to Applied Regression" (which is a really great book, I'll add), and it has a lot of tools that I'm quite fond of. The `car` package was written by a guy called John Fox, who has written a lot of great statistical tools for social science applications. The `psych` package was written by William Revelle, and it has a lot of functions that are very useful for psychologists in particular, especially in regards to psychometric techniques. For the most part, `car` and `psych` are quite unrelated to each other. They do different things, so not surprisingly almost all of the function names are different. But... there's one exception to that. The `car` package and the `psych` package *both* contain a function called `logit()`.[^logit] This creates a naming conflict. If I load both packages into Python, an ambiguity is created. If the user types in `logit(100)`, should Python use the `logit()` function in the `car` package, or the one in the `psych` package? The answer is: Python uses whichever package you loaded most recently, and it tells you this very explicitly. Here's what happens when I load the `car` package, and then afterwards load the `psych` package: 
```{r}
library(car)
library(psych)
```
The output here is telling you that the `logit` object (i.e., function) in the `car` package is no longer accessible to you. It's been hidden (or "masked") from you by the one in the `psych` package.[^double_colon]

### Downloading new packages

One of the main selling points for Python is that there are thousands of packages that have been written for it, and these are all available online. So whereabouts online are these packages to be found, and how do we download and install them? There is a big repository of packages called the "Comprehensive Python Archive Network" (CRAN), and the easiest way of getting and installing a new package is from one of the many CRAN mirror sites. Conveniently for us, Python provides a function called `install.packages()` that you can use to do this. Even *more* conveniently, the RStudio team runs its own CRAN mirror and RStudio has a clean interface that lets you install packages without having to learn how to use the `install.packages()` command[^difficult]

Using the RStudio tools is, again, dead simple. In the top left hand corner of the packages panel (Figure \@ref(fig:packagepanel)) you'll see a button called "Install Packages". If you click on that, it will bring up a window like the one shown in Figure \@ref(fig:packageinstalla).

```{r packageinstalla, fig.cap=c("The package installation dialog box in RStudio"), echo=FALSE}

knitr::include_graphics(here::here("bookdown", "img", "mechanics", "installpackage.png"))

```

There are a few different buttons and boxes you can play with. Ignore most of them. Just go to the line that says "Packages" and start typing the name of the package that you want. As you type, you'll see a dropdown menu appear (Figure \@ref(fig:packageinstallb)), listing names of packages that start with the letters that you've typed so far. 

```{r packageinstallb, fig.cap="When you start typing, you\'ll see a dropdown menu suggest a list of possible packages that you might want to install", echo=FALSE}

knitr::include_graphics(here::here("bookdown", "img", "mechanics", "installpackage2.png"))

```

You can select from this list, or just keep typing. Either way, once you've got the package name that you want, click on the install button at the bottom of the window. When you do, you'll see the following command appear in the Python console:
```{r eval=FALSE}
install.packages("psych")
```
This is the Python command that does all the work. Python then goes off to the internet, has a conversation with CRAN, downloads some stuff, and installs it on your computer. You probably don't care about all the details of Python's little adventure on the web, but the `install.packages()` function is rather chatty, so it reports a bunch of gibberish that you really aren't all that interested in:
```
trying URL 'http://cran.rstudio.com/bin/macosx/contrib/3.0/psych_1.4.1.tgz'
Content type 'application/x-gzip' length 2737873 bytes (2.6 Mb)
opened URL
==================================================
downloaded 2.6 Mb


The downloaded binary packages are in
	/var/folders/cl/thhsyrz53g73q0w1kb5z3l_80000gn/T//RtmpmQ9VT3/downloaded_packages
```
Despite the long and tedious response, all thar really means is "I've installed the psych package". I find it best to humour the talkative little automaton. I don't actually read any of this garbage, I just politely say "thanks" and go back to whatever I was doing.





### Updating Python and Python packages

Every now and then the authors of packages release updated versions. The updated versions often add new functionality, fix bugs, and so on. It's generally a good idea to update your packages periodically. There's an `update.packages()` function that you can use to do this, but it's probably easier to stick with the RStudio tool. In the packages panel, click on the "Update Packages" button. This will bring up a window that looks like the one shown in Figure \@ref(fig:updatepackages). In this window, each row refers to a package that needs to be updated. You can to tell Python which updates you want to install by checking the boxes on the left. If you're feeling lazy and just want to update everything, click the "Select All" button, and then click the "Install Updates" button. Python then prints out a *lot* of garbage on the screen, individually downloading and installing all the new packages. This might take a while to complete depending on how good your internet connection is. Go make a cup of coffee. Come back, and all will be well. 

```{r updatepackages, fig.cap="The RStudio dialog box for updating packages", echo=FALSE}
knitr::include_graphics(here::here("bookdown", "img", "mechanics", "updatepackages.png"))
```

About every six months or so, a new version of Python is released. You can't update Python from within RStudio (not to my knowledge, at least): to get the new version you can go to the CRAN website and download the most recent version of Python, and install it in the same way you did when you originally installed Python on your computer. This used to be a slightly frustrating event, because whenever you downloaded the new version of Python, you would lose all the packages that you'd downloaded and installed, and would have to repeat the process of re-installing them. This was pretty annoying, and there were some neat tricks you could use to get around this. However, newer versions of Python don't have this problem so I no longer bother explaining the workarounds for that issue.

### What packages does this book use?


There are several packages that I make use of in this book. The most prominent ones are:


- `lsr`. This is the *Learning Statistics with Python* package that accompanies this book. It doesn't have a lot of interesting high-powered tools: it's just a small collection of handy little things that I think can be useful to novice users. As you get more comfortable with Python this package should start to feel pretty useless to you.
- `psych`. This package, written by William Revelle, includes a lot of tools that are of particular use to psychologists. In particular, there's several functions that are particularly convenient for producing analyses or summaries that are very common in psych, but less common in other disciplines. 
- `car`. This is the *Companion to Applied Regression* package, which accompanies the excellent book of the same name by (Fox and Weisberg 2011){cite}`Fox2011`. It provides a lot of very powerful tools, only some of which we'll touch in this book.


 
Besides these three, there are a number of packages that I use in a more limited fashion: `gplots`, `sciplot`, `foreign`, `effects`, `R.matlab`, `gdata`, `lmtest`, and probably one or two others that I've missed. There are also a number of packages that I refer to but don't actually use in this book, such as `reshape`, `compute.es`, `HistData` and `multcomp` among others. Finally, there are a number of packages that provide more advanced tools that I hope to talk about in future versions of the book, such as `sem`, `ez`, `nlme` and `lme4`. In any case, whenever I'm using a function that isn't in the core packages, I'll make sure to note this in the text.



(workspace)=
## Managing the workspace

Let's suppose that you're reading through this book, and what you're doing is sitting down with it once a week and working through a whole chapter in each sitting. Not only that, you've been following my advice and typing in all these commands into Python. So far during this chapter, you'd have typed quite a few commands, although the only ones that actually involved creating variables were the ones you typed during Section \@ref(comments). As a result, you currently have three variables; `seeker`, `lover`, and `keeper`. These three variables are the contents of your **_workspace_**, also referred to as the **global environment**. The workspace is a key concept in Python, so in this section we'll talk a lot about what it is and how to manage its contents.



### Listing the contents of the workspace


```{r workspace, fig.cap="The RStudio Environment panel shows you the contents of the workspace. The view shown above is the list view. To switch to the grid view, click on the menu item on the top right that currently reads list. Select grid from the dropdown menu, and then it will switch to a view like the one shown in the other workspace figure", echo=FALSE}
knitr::include_graphics(here::here("bookdown", "img", "mechanics", "workspacepanel.png"))

```

```{r workspace2, fig.cap="The RStudio \"Environment\" panel shows you the contents of the workspace. Compare this \"grid\" view to the \"list\" earlier", echo=FALSE}
knitr::include_graphics(here::here("bookdown", "img", "mechanics", "workspacepanel2.png"))

```


The first thing that you need to know how to do is examine the contents of the workspace. If you're using RStudio, you will probably find that the easiest way to do this is to use the "Environment" panel in the top right hand corner. Click on that, and you'll see a list that looks very much like the one shown in Figures \@ref(fig:workspace) and \@ref(fig:workspace2). If you're using the commmand line, then the `objects()` function may come in handy:
```{r}
objects()
```
Of course, in the true Python tradition, the `objects()` function has a lot of fancy capabilities that I'm glossing over in this example. Moreover there are also several other functions that you can use, including `ls()` which is pretty much identical to `objects()`, and `ls.str()` which you can use to get a fairly detailed description of all the variables in the workspace. In fact, the `lsr` package actually includes its own function that you can use for this purpose, called `who()`. The reason for using the `who()` function is pretty straightforward: in my everyday work I find that the output produced by the `objects()` command isn't *quite* informative enough, because the only thing it prints out is the name of each variable; but the `ls.str()` function is *too* informative, because it prints out a lot of additional information that I really don't like to look at. The `who()` function is a compromise between the two. First, now that we've got the `lsr` package installed, we need to load it:
```{r}
library(lsr)
```
and now we can use the `who()` function:
```{r}
who()
```
As you can see, the `who()` function lists all the variables and provides some basic information about what kind of variable each one is and how many elements it contains. Personally, I find this output much easier more useful than the very compact output of the `objects()` function, but less overwhelming than the extremely verbose `ls.str()` function. Throughout this book you'll see me using the `who()` function a lot. You don't have to use it yourself: in fact, I suspect you'll find it easier to look at the RStudio environment panel. But for the purposes of writing a textbook I found it handy to have a nice text based description: otherwise there would be about another 100 or so screenshots added to the book.[^who]


### Removing variables from the workspace

Looking over that list of variables, it occurs to me that I really don't need them any more. I created them originally just to make a point, but they don't serve any useful purpose anymore, and now I want to get rid of them. I'll show you how to do this, but first I want to warn you -- there's no "undo" option for variable removal. Once a variable is removed, it's gone forever unless you save it to disk. I'll show you how to do *that* in Section \@ref(load), but quite clearly we have no need for these variables at all, so we can safely get rid of them.

In RStudio, the easiest way to remove variables is to use the environment panel. Assuming that you're in grid view (i.e., Figure \@ref(fig:workspace2)), check the boxes next to the variables that you want to delete, then click on the "Clear" button at the top of the panel. When you do this, RStudio will show a dialog box asking you to confirm that you really do want to delete the variables. It's always worth checking that you really do, because as RStudio is at pains to point out, you can't undo this. Once a variable is deleted, it's gone.[^removed] In any case, if you click "yes", that variable will disappear from the workspace: it will no longer appear in the environment panel, and it won't show up when you use the `who()` command.

Suppose you don't access to RStudio, and you still want to remove variables. This is where the **_remove_** function `rm()` comes in handy. The simplest way to use `rm()` is just to type in a (comma separated) list of all the variables you want to remove. Let's say I want to get rid of `seeker` and `lover`, but I would like to keep `keeper`. To do this, all I have to do is type:
```{r}
rm( seeker, lover )
```
There's no visible output, but if I now inspect the workspace
```{r}
who()
```
I see that there's only the `keeper` variable left. As you can see, `rm()` can be very handy for keeping the workspace tidy. 






(navigation)=
## Navigating the file system


In this section I talk a little about how Python interacts with the file system on your computer. It's not a terribly interesting topic, but it's useful. As background to this discussion, I'll talk a bit about how file system locations work in Section \@ref(filesystem). Once upon a time *everyone* who used computers could safely be assumed to understand how the file system worked, because it was impossible to successfully use a computer if you didn't! However, modern operating systems are much more "user friendly", and as a consequence of this they go to great lengths to hide the file system from users. So these days it's not at all uncommon for people to have used computers most of their life and not be familiar with the way that computers organise files. If you already know this stuff, skip straight to Section \@ref(navigationPython). Otherwise, read on. I'll try to give a brief introduction that will be useful for those of you who have never been forced to learn how to navigate around a computer using a DOS or UNIX shell. 

(filesystem)=
### The file system itself

In this section I describe the basic idea behind file locations and file paths. Regardless of whether you're using Window, Mac OS or Linux, every file on the computer is assigned a (fairly) human readable address, and every address has the same basic structure: it describes a *path* that starts from a *root* location , through as series of *folders* (or if you're an old-school computer user, *directories*), and finally ends up at the file. 

On a Windows computer the root is the physical drive[^partition] on which the file is stored, and for most home computers the name of the hard drive that stores all your files is C: and therefore most file names on Windows begin with C:. After that comes the folders, and on Windows the folder names are separated by a `\` symbol. So, the complete path to this book on my Windows computer might be something like this:
```
C:\Users\danPython\LSPython.pdf
```
and what that *means* is that the book is called LSPython.pdf, and it's in a folder called `book` which itself is in a folder called dan which itself is ... well, you get the idea. On Linux, Unix and Mac OS systems, the addresses look a little different, but they're more or less identical in spirit. Instead of using the backslash, folders are separated using a forward slash, and unlike Windows, they don't treat the physical drive as being the root of the file system. So, the path to this book on my Mac might be something like this:
```
/Users/dan/Python/LSPython.pdf
```

So that's what we mean by the "path" to a file. The next concept to grasp is the idea of a **_working directory_** and how to change it. For those of you who have used command line interfaces previously, this should be obvious already. But if not, here's what I mean. The working directory is just "whatever folder I'm currently looking at". Suppose that I'm currently looking for files in Explorer (if you're using Windows) or using Finder (on a Mac). The folder I currently have open is my user directory (i.e., `C:\Users\dan` or `/Users/dan`). That's my current working directory. 

The fact that we can imagine that the program is "in" a particular directory means that we can talk about moving *from* our current location *to* a new one. What that means is that we might want to specify a new location in relation to our current location. To do so, we need to introduce two new conventions. Regardless of what operating system you're using, we use `.` to refer to the current working directory, and `..` to refer to the directory above it. This allows us to specify a path to a new location in relation to our current location, as the following examples illustrate. Let's assume that I'm using my Windows computer, and my working directory is `C:\Users\danPython`). The table below shows several addresses in relation to my current one:


```{r echo=FALSE}
knitr::kable(rbind(
                  c("C:\\\\Users\\\\dan" , ".."),
                  c("C:\\\\Users ", "..\\\\.. \\\\"),
                  c("C:\\\\Users\\\\danPython\\\\source ", ".\\\\source "),
                  c("C:\\\\Users\\\\dan\\\\nerdstuff ", "..\\\\nerdstuff")),
caption = 'Basic arithmetic operations in Python. These five operators are used very frequently throughout the text, so it\'s important to be familiar with them at the outset.',col.names = c("absolute path (i.e., from root)"  , "relative path (i.e. from C:\\Users\\danPython)"),
  booktabs = TRUE)
```

There's one last thing I want to call attention to: the `~` directory. I normally wouldn't bother, but Python makes reference to this concept sometimes. It's quite common on computers that have multiple users to define `~` to be the user's home directory. On my Mac, for instance, the home directory `~` for the "dan" user is `\Users\dan\`. And so, not surprisingly, it is possible to define other directories in terms of their relationship to the home directory. For example, an alternative way to describe the location of the `LSPython.pdf` file on my Mac would be 

```
~Rbook\LSPython.pdf
```


That's about all you really need to know about file paths. And since this section already feels too long, it's time to look at how to navigate the file system in Python. 

(navigationPython)=
### Navigating the file system using the Python console

In this section I'll talk about how to navigate this file system from within Python itself. It's not particularly user friendly, and so you'll probably be happy to know that RStudio provides you with an easier method, and I will describe it in Section \@ref(nav3). So in practice, you won't *really* need to use the commands that I babble on about in this section, but I do think it helps to see them in operation at least once before forgetting about them forever.

Okay, let's get started. When you want to load or save a file in Python it's important to know what the working directory is. You can find out by using the `getwd()` command. For the moment, let's assume that I'm using Mac OS or Linux, since there's some subtleties to Windows. Here's what happens:
```
getwd()
## [1] "/Users/dan"
```
We can change the working directory quite easily using `setwd()`. The `setwd()` function has only the one argument, `dir`, is a character string specifying a path to a directory, or a path relative to the working directory. Since I'm currently located at `/Users/dan`, the following two are equivalent: 
```
setwd("/Users/dan/Rbook/data")
setwd("./Rbook/data")
```
Now that we're here, we can type `list.files()` command to get a listing of all the files in that directory. Since this is the directory in which I store all of the data files that we'll use in this book, here's what we get as the result:
```
list.files()
## [1] "afl24.Rdata"             "aflsmall.Rdata"          "aflsmall2.Rdata"        
## [4] "agpp.Rdata"              "all.zip"                 "annoying.Rdata"         
## [7] "anscombesquartet.Rdata"  "awesome.Rdata"           "awesome2.Rdata"         
## [10] "booksales.csv"           "booksales.Rdata"         "booksales2.csv"         
## [13] "cakes.Rdata"             "cards.Rdata"             "chapek9.Rdata"          
## [16] "chico.Rdata"             "clinicaltrial_old.Rdata" "clinicaltrial.Rdata"    
## [19] "coffee.Rdata"            "drugs.wmc.rt.Rdata"      "dwr_all.Rdata"          
## [22] "effort.Rdata"            "happy.Rdata"             "harpo.Rdata"            
## [25] "harpo2.Rdata"            "likert.Rdata"            "nightgarden.Rdata"      
## [28] "nightgarden2.Rdata"      "parenthood.Rdata"        "parenthood2.Rdata"      
## [31] "randomness.Rdata"        "repeated.Rdata"          "rtfm.Rdata"             
## [34] "salem.Rdata"             "zeppo.Rdata"
```
Not terribly exciting, I'll admit, but it's useful to know about. In any case, there's only one more thing I want to make a note of, which is that Python also makes use of the home directory. You can find out what it is by using the `path.expand()` function, like this:
```
path.expand("~")
## [1] "/Users/dan"
```
You can change the user directory if you want, but we're not going to make use of it very much so there's no reason to. The only reason I'm even bothering to mention it at all is that when you use RStudio to open a file, you'll see output on screen that defines the path to the file relative to the `~` directory. I'd prefer you not to be confused when you see it.[^choose]




### Why do the Windows paths use the wrong slash?

Let's suppose I'm on Windows. As before, I can find out what my current working directory is like this:
```
getwd()
## [1] "C:/Users/dan/
```
This seems about right, but you might be wondering why Python is displaying a Windows path using the wrong type of slash. The answer is slightly complicated, and has to do with the fact that Python treats the `\` character as "special" (see Section \@ref(escapechars)). If you're deeply wedded to the idea of specifying a path using the Windows style slashes, then what you need to do is to type `/` whenever you mean `\`. In other words, if you want to specify the working directory on a Windows computer, you need to use one of the following commands:
```
setwd( "C:/Users/dan" )
setwd( "C:\\Users\\dan" )
```
It's kind of annoying to have to do it this way, but as you'll see later on in Section \@ref(escapechars) it's a necessary evil. Fortunately, as we'll see in the next section, RStudio provides a much simpler way of changing directories...

(nav3)=
### Navigating the file system using the RStudio file panel

Although I think it's important to understand how all this command line stuff works, in many (maybe even most) situations there's an easier way. For our purposes, the easiest way to navigate the file system is to make use of RStudio's built in tools. The "file" panel -- the lower right hand area in Figure \@ref(fig:filepanel) -- is actually a pretty decent file browser. Not only can you just point and click on the names to move around the file system, you can also use it to set the working directory, and even load files. 

```{r filepanel, fig.cap="The \"file panel\" is the area shown in the lower right hand corner. It provides a very easy way to browse and navigate your computer using Python. See main text for details.", echo=FALSE}

knitr::include_graphics(here::here("bookdown", "img", "mechanics", "filepanel.png"))

```


Here's what you need to do to change the working directory using the file panel. Let's say I'm looking at the actual screen shown in Figure \@ref(fig:filepanel). At the top of the file panel you see some text that says "Home $>$ Rbook $>$ data". What that means is that it's *displaying* the files that are stored in the 
```
/Users/dan/Rbook/data
```
directory on my computer. It does *not* mean that this is the Python working directory. If you want to change the Python working directory, using the file panel, you need to click on the button that reads "More". This will bring up a little menu, and one of the options will be "Set as Working Directory". If you select that option, then Python really will change the working directory. You can tell that it has done so because this command appears in the console: 
```
setwd("~/Rbook/data")
```
In other words, RStudio sends a command to the Python console, exactly as if you'd typed it yourself. The file panel can be used to do other things too. If you want to move "up" to the parent folder (e.g., from `/Users/dan/Rbook/data` to `/Users/dan/Rbook` click on the ".." link in the file panel. To move to a subfolder, click on the name of the folder that you want to open. You can open some types of file by clicking on them. You can delete files from your computer using the "delete" button, rename them with the "rename" button, and so on.


As you can tell, the file panel is a very handy little tool for navigating the file system. But it can do more than just navigate. As we'll see later, it can be used to open files. And if you look at the buttons and menu options that it presents, you can even use it to rename, delete, copy or move files, and create new folders. However, since most of that functionality isn't critical to the basic goals of this book, I'll let you discover those on your own.



(load)=
## Loading and saving data


There are several different types of files that are likely to be relevant to us when doing data analysis. There are three in particular that are especially important from the perspective of this book:

- *Workspace files* are those with a .Rdata file extension. This is the standard kind of file that Python uses to store data and variables. They're called "workspace files" because you can use them to save your whole workspace. 
- *Comma separated value (CSV) files* are those with a .csv file extension. These are just regular old text files, and they can be opened with almost any software. It's quite typical for people to store data in CSV files, precisely because they're so simple.
- *Script files* are those with a .Python file extension. These aren't data files at all; rather, they're used to save a collection of commands that you want Python to execute later. They're just text files, but we won't make use of them until Chapter \@ref(scripting).
 
There are also several other types of file that Python makes use of,[^r_files] but they're not really all that central to our interests. There are also several other kinds of data file that you might want to import into Python. For instance, you might want to open Microsoft Excel spreadsheets (.xlsx files), or data files that have been saved in the native file formats for other statistics software, such as SPSS, SAS, Minitab, Stata or Systat. Finally, you might have to handle databases. Python tries hard to play nicely with other software, so it has tools that let you open and work with any of these and many others. I'll discuss some of these other possibilities elsewhere in this book (Section \@ref(importing)), but for now I want to focus primarily on the two kinds of data file that you're most likely to need: .Rdata files and .csv files.
In this section I'll talk about how to load a workspace file, how to import data from a CSV file, and how to save your workspace to a workspace file. Throughout this section I'll first describe the (sometimes awkward) Python commands that do all the work, and then I'll show you the (much easier) way to do it using RStudio.



### Loading workspace files using Python

When I used the `list.files()` command to list the contents of the `/Users/dan/Rbook/data` directory (in Section \@ref(navigationPython)), the output referred to a file called booksales.Rdata. Let's say I want to load the data from this file into my workspace. The way I do this is with the `load()` function. There are two arguments to this function, but the only one we're interested in is

- `file`. This should be a character string that specifies a path to the file that needs to be loaded. You can use an absolute path or a relative path to do so.

Using the absolute file path, the command would look like this:
```
load( file = "/Users/dan/Rbook/data/booksales.Rdata" )
```
but this is pretty lengthy. Given that the working directory (remember, we changed the directory at the end of Section \@ref(nav3)) is `/Users/dan/Rbook/data`, I could use a relative file path, like so:
```
load( file = "../data/booksales.Rdata" )
```
However, my preference is usually to change the working directory first, and *then* load the file. What that would look like is this:
```
setwd( "../data" )         # move to the data directory
load( "booksales.Rdata" )  # load the data
```
If I were then to type `who()` I'd see that there are several new variables in my workspace now. Throughout this book, whenever you see me loading a file, I will assume that the file is actually stored in the working directory, or that you've changed the working directory so that Python is pointing at the directory that contains the file. Obviously, *you* don't need type that command yourself: you can use the RStudio file panel to do the work.

### Loading workspace files using RStudio

Okay, so how do we open an .Rdata file using the RStudio file panel? It's terribly simple. First, use the file panel to find the folder that contains the file you want to load. If you look at Figure \@ref(fig:filepanel), you can see that there are several .Rdata files listed. Let's say I want to load the `booksales.Rdata` file. All I have to do is click on the file name. RStudio brings up a little dialog box asking me to confirm that I do want to load this file. I click yes. The following command then turns up in the console,
```
load("~/Rbook/data/booksales.Rdata")
```
and the new variables will appear in the workspace (you'll see them in the Environment panel in RStudio, or if you type `who()`). So easy it barely warrants having its own section.


(loadingcsv)=
### Importing data from CSV files using loadingcsv

One quite commonly used data format is the humble "comma separated value" file, also called a CSV file, and usually bearing the file extension .csv. CSV files are just plain old-fashioned text files, and what they store is basically just a table of data. This is illustrated in Figure \@ref(fig:booksalescsv), which shows a file called booksales.csv that I've created. As you can see, each row corresponds to a variable, and each row represents the book sales data for one month. The first row doesn't contain actual data though: it has the names of the variables.

```{r booksalescsv, fig.cap="The booksales.csv data file. On the left, I've opened the file in using a spreadsheet program (OpenOffice), which shows that the file is basically a table. On the right, the same file is open in a standard text editor (the TextEdit program on a Mac), which shows how the file is formatted. The entries in the table are wrapped in quote marks and separated by commas.", echo=FALSE}

knitr::include_graphics(here::here("bookdown", "img", "mechanics", "booksalescsv.jpg"))

```

If RStudio were not available to you, the easiest way to open this file would be to use the `read.csv()` function.[^read_table] This function is pretty flexible, and I'll talk a lot more about it's capabilities in Section \@ref(importing) for more details, but for now there's only two arguments to the function that I'll mention:

- `file`. This should be a character string that specifies a path to the file that needs to be loaded. You can use an absolute path or a relative path to do so.
- `header`. This is a logical value indicating whether or not the first row of the file contains variable names. The default value is `TRUE`. 

Therefore, to import the CSV file, the command I need is:

```{r echo=FALSE}
books <- read.csv( file = "./data/booksales.csv" )
```

```{r eval=FALSE}
books <- read.csv( file = "booksales.csv" )
```
There are two very important points to notice here. Firstly, notice that I *didn't* try to use the `load()` function, because that function is only meant to be used for .Rdata files. If you try to use `load()` on other types of data, you get an error. Secondly, notice that when I imported the CSV file I assigned the result to a variable, which I imaginatively called `books`.[^loading] file. There's a reason for this. The idea behind an `.Rdata` file is that it stores a whole workspace. So, if you had the ability to look inside the file yourself you'd see that the data file keeps track of all the variables and their names. So when you `load()` the file, Python restores all those original names. CSV files are treated differently: as far as Python is concerned, the CSV only stores *one* variable, but that variable is big table. So when you import that table into the workspace, Python expects *you* to give it a name.] Let's have a look at what we've got:
```{r}
print( books )
```
Clearly, it's worked, but the format of this output is a bit unfamiliar. We haven't seen anything like this before. What you're looking at is a *data frame*, which is a very important kind of variable in Python, and one I'll discuss in Section \@ref(dataframes). For now, let's just be happy that we imported the data and that it looks about right.


### Importing data from CSV files using RStudio

Yet again, it's easier in RStudio. In the environment panel in RStudio you should see a button called "Import Dataset". Click on that, and it will give you a couple of options: select the "From Text File..." option, and it will open up a very familiar dialog box asking you to select a file: if you're on a Mac, it'll look like the usual Finder window that you use to choose a file; on Windows it looks like an Explorer window. An example of what it looks like on a Mac is shown in Figure \@ref(fig:fileopen). I'm assuming that you're familiar with your own computer, so you should have no problem finding the CSV file that you want to import! Find the one you want, then click on the "Open" button. When you do this, you'll see a window that looks like the one in Figure \@ref(fig:import).

```{r fileopen, fig.cap="A dialog box on a Mac asking you to select the CSV file Python should try to import. Mac users will recognise this immediately: it's the usual way in which a Mac asks you to find a file. Windows users won't see this: they'll see the usual explorer window that Windows always gives you when it wants you to select a file.", echo=FALSE}

knitr::include_graphics(here::here("bookdown", "img", "mechanics", "openscreen.png"))

```





The import data set window is relatively straightforward to understand. 

```{r import, fig.cap="The RStudio window for importing a CSV file into Python", echo=FALSE}

knitr::include_graphics(here::here("bookdown", "img", "mechanics", "import.png"))

```

In the top left corner, you need to type the name of the variable you Python to create. By default, that will be the same as the file name: our file is called `booksales.csv`, so RStudio suggests the name `booksales`. If you're happy with that, leave it alone. If not, type something else. Immediately below this are a few things that you can tweak to make sure that the data gets imported correctly: 


- Heading. Does the first row of the file contain raw data, or does it contain headings for each variable? The `booksales.csv` file has a header at the top, so I selected "yes".
- Separator. What character is used to separate different entries? In most CSV files this will be a comma (it is "comma separated" after all). But you can change this if your file is different. 
- Decimal. What character is used to specify the decimal point? In English speaking countries, this is almost always a period (i.e., `.`). That's not universally true: many European countries use a comma. So you can change that if you need to.
- Quote. What character is used to denote a block of text? That's usually going to be a double quote mark. It is for the `booksales.csv` file, so that's what I selected.

The nice thing about the RStudio window is that it shows you the raw data file at the top of the window, and it shows you a preview of the data at the bottom. If the data at the bottom doesn't look right, try changing some of the settings on the left hand side. Once you're happy, click "Import". When you do, two commands appear in the Python console:
```
booksales <- read.csv("~/Rbook/data/booksales.csv")
View(booksales)
```
The first of these commands is the one that loads the data. The second one will display a pretty table showing the data in RStudio. 


### Saving a workspace file using `save`

Not surprisingly, saving data is very similar to loading data. Although RStudio provides a simple way to save files (see below), it's worth understanding the actual commands involved. There are two commands you can use to do this, `save()` and `save.image()`. If you're happy to save *all* of the variables in your workspace into the data file, then you should use `save.image()`. And if you're happy for Python to save the file into the current working directory, all you have to do is this:
```{r eval=FALSE}
save.image( file = "myfile.Rdata" )
```
Since `file` is the first argument, you can shorten this to `save.image("myfile.Rdata")`; and if you want to save to a different directory, then (as always) you need to be more explicit about specifying the path to the file, just as we discussed in Section \@ref(navigation). Suppose, however, I have several variables in my workspace, and I only want to save some of them. For instance, I might have this as my workspace:
```{r eval=FALSE}
who()
##   -- Name --   -- Class --   -- Size --
##   data         data.frame    3 x 2     
##   handy        character     1         
##   junk         numeric       1        
```
I want to save `data` and `handy`, but not `junk`. But I don't want to delete `junk` right now, because I want to use it for something else later on. This is where the `save()` function is useful, since it lets me indicate exactly which variables I want to save. Here is one way I can use the `save` function to solve my problem:
```{r eval=FALSE}
save(data, handy, file = "myfile.Rdata")
```
Importantly, you *must* specify the name of the `file` argument. The reason is that if you don't do so, Python will think that `"myfile.Rdata"` is actually a *variable* that you want to save, and you'll get an error message. Finally, I should mention a second way to specify which variables the `save()` function should save, which is to use the `list` argument. You do so like this:
```{r eval = FALSE}
save.me <- c("data", "handy")   # the variables to be saved
save( file = "booksales2.Rdata", list = save.me )   # the command to save them
```

(save1)=
### Saving a workspace file using RStudio

RStudio allows you to save the workspace pretty easily. In the environment panel (Figures \@ref(fig:workspace) and \@ref(fig:workspace2)) you can see the "save" button. There's no text, but it's the same icon that gets used on every computer everywhere: it's the one that looks like a floppy disk. You know, those things that haven't been used in about 20 years. Alternatively, go to the "Session" menu and click on the "Save Workspace As..." option.[^saving] This will bring up the standard "save" dialog box for your operating system (e.g., on a Mac it'll look a little bit like the loading dialog box in Figure \@ref(fig:fileopen)). Type in the name of the file that you want to save it to, and all the variables in your workspace will be saved to disk. You'll see an Python command like this one
```{r eval=FALSE}
save.image("~/Desktop/Untitled.RData")
```
Pretty straightforward, really.



### Other things you might want to save

Until now, we've talked mostly about loading and saving *data*. Other things you might want to save include:



- *The output*. Sometimes you might also want to keep a copy of all your interactions with Python, including everything that you typed in and everything that Python did in response. There are some functions that you can use to get Python to write its output to a file rather than to print onscreen (e.g., `sink()`), but to be honest, if you do want to save the Python output, the easiest thing to do is to use the mouse to select the relevant text in the Python console, go to the "Edit" menu in RStudio and select "Copy". The output has now been copied to the clipboard. Now open up your favourite text editor or word processing software, and paste it. And you're done. However, this will only save the contents of the console, not the plots you've drawn (assuming you've drawn some). We'll talk about saving images later on.


- *A script*. While it is possible -- and sometimes handy -- to save the Python output as a method for keeping a copy of your statistical analyses, another option that people use a lot (especially when you move beyond simple "toy" analyses) is to write *scripts*. A script is a text file in which you write out all the commands that you want Python to run. You can write your script using whatever software you like. In real world data analysis writing scripts is a key skill -- and as you become familiar with Python you'll probably find that most of what you do involves scripting rather than typing commands at the Python prompt. However, you won't need to do much scripting initially, so we'll leave that until Chapter \@ref(scripting).






(useful)=
## Useful things to know about variables

In Chapter \@ref(introPython) I talked a lot about variables, how they're assigned and some of the things you can do with them, but there's a lot of additional complexities. That's not a surprise of course. However, some of those issues are worth drawing your attention to now. So that's the goal of this section; to cover a few extra topics. As a consequence, this section is basically a bunch of things that I want to briefly mention, but don't really fit in anywhere else. In short, I'll talk about several different issues in this section, which are only loosely connected to one another.



(specials)=
### Special values

The first thing I want to mention are some of the "special" values that you might see Python produce. Most likely you'll see them in situations where you were expecting a number, but there are quite a few other ways you can encounter them. These values are `Inf`, `NaN`, `NA` and `NULL`. These values can crop up in various different places, and so it's important to understand what they mean. 


- *Infinity* (`Inf`). The easiest of the special values to explain is `Inf`, since it corresponds to a value that is infinitely large. You can also have `-Inf`. The easiest way to get `Inf` is to divide a positive number by 0:
```{r}
1 / 0
```
In most real world data analysis situations, if you're ending up with infinite numbers in your data, then something has gone awry. Hopefully you'll never have to see them.


- *Not a Number* (`NaN`). The special value of `NaN` is short for "not a number", and it's basically a reserved keyword that means "there isn't a mathematically defined number for this". If you can remember your high school maths, remember that it is conventional to say that $0/0$ doesn't have a proper answer: mathematicians would say that $0/0$ is *undefined*. Python says that it's not a number:
```{r}
 0 / 0
```
Nevertheless, it's still treated as a "numeric" value. To oversimplify, `NaN` corresponds to cases where you asked a proper numerical question that genuinely has *no meaningful answer*. 

- *Not available* (`NA`). 
`NA` indicates that the value that is "supposed" to be stored here is missing. To understand what this means, it helps to recognise that the `NA` value is something that you're most likely to see when analysing data from real world experiments. Sometimes you get equipment failures, or you lose some of the data, or whatever. The point is that some of the information that you were "expecting" to get from your study is just plain missing. Note the difference between `NA` and `NaN`. For `NaN`, we really do know what's supposed to be stored; it's just that it happens to correspond to something like $0/0$ that doesn't make any sense at all. In contrast, `NA` indicates that we actually don't know what was supposed to be there. The information is *missing*.

- *No value* (`NULL`).
The `NULL` value takes this "absence" concept even further. It basically asserts that the variable genuinely has no value whatsoever. This is quite different to both `NaN` and `NA`. For `NaN` we actually know what the value is, because it's something insane like $0/0$. For `NA`, we believe that there is supposed to be a value "out there", but a dog ate our homework and so we don't quite know what it is. But for `NULL` we strongly believe that there is *no value at all*.


(names)=
### Assigning names to vector elements

One thing that is sometimes a little unsatisfying about the way that Python prints out a vector is that the elements come out unlabelled. Here's what I mean. Suppose I've got data reporting the quarterly profits for some company. If I just create a no-frills vector, I have to rely on memory to know which element corresponds to which event. That is:
```{r}
profit <- c( 3.1, 0.1, -1.4, 1.1 )
profit

```
You can probably guess that the first element corresponds to the first quarter, the second element to the second quarter, and so on, but that's only because I've told you the back story and because this happens to be a very simple example. In general, it can be quite difficult. This is where it can be helpful to assign `names` to each of the elements. Here's how you do it:
```{r}
names(profit) <- c("Q1","Q2","Q3","Q4")
profit
```
This is a slightly odd looking command, admittedly, but it's not too difficult to follow. All we're doing is assigning a vector of labels (character strings) to `names(profit)`. You can always delete the names again by using the command `names(profit) <- NULL`. It's also worth noting that you don't have to do this as a two stage process. You can get the same result with this command:
```{r}
profit <- c( "Q1" = 3.1, "Q2" = 0.1, "Q3" = -1.4, "Q4" = 1.1 )
profit
```
The important things to notice are that (a) this does make things much easier to read, but (b) the names at the top aren't the "real" data. The *value* of `profit[1]` is still `3.1`; all I've done is added a *name* to `profit[1]` as well. Nevertheless, names aren't purely cosmetic, since Python allows you to pull out particular elements of the vector by referring to their names:
```{r}
profit["Q1"]
```
And if I ever need to pull out the names themselves, then I just type `names(profit)`. 

### Variable classes

As we've seen, Python allows you to store different kinds of data. In particular, the variables we've defined so far have either been character data (text), numeric data, or logical data.[^functions] It's important that we remember what kind of information each variable stores (and even more important that Python remembers) since different kinds of variables allow you to do different things to them. For instance, if your variables have numerical information in them, then it's okay to multiply them together:
```{r}
x <- 5   # x is numeric
y <- 4   # y is numeric
x * y    
```
But if they contain character data, multiplication makes no sense whatsoever, and Python will complain if you try to do it:
```{r error=TRUE}
x <- "apples"   # x is character
y <- "oranges"  # y is character
x * y           
```
Even Python is smart enough to know you can't multiply `"apples"` by `"oranges"`. It knows this because the quote marks are indicators that the variable is supposed to be treated as text, not as a number. 

This is quite useful, but notice that it means that Python makes a big distinction between `5` and `"5"`. Without quote marks, Python treats `5` as the number five, and will allow you to do calculations with it. With the quote marks, Python treats `"5"` as the textual character five, and doesn't recognise it as a number any more than it recognises `"p"` or `"five"` as numbers. As a consequence, there's a big difference between typing `x <- 5` and typing `x <- "5"`. In the former, we're storing the number `5`; in the latter, we're storing the character `"5"`. Thus, if we try to do multiplication with the character versions, Python gets stroppy:
```{r error=TRUE}
x <- "5"   # x is character
y <- "4"   # y is character
x * y     

```

Okay, let's suppose that I've forgotten what kind of data I stored in the variable `x` (which happens depressingly often). Python provides a function that will let us find out. Or, more precisely, it provides *three* functions: `class()`, `mode()` and `typeof()`. Why the heck does it provide three functions, you might be wondering? Basically, because Python actually keeps track of three different kinds of information about a variable:

1. The **_class_** of a variable is a "high level" classification, and it captures psychologically (or statistically) meaningful distinctions. For instance `"2011-09-12"` and `"my birthday"` are both text strings, but there's an important difference between the two: one of them is a date. So it would be nice if we could get Python to recognise that `"2011-09-12"` is a date, and allow us to do things like add or subtract from it. The class of a variable is what Python uses to keep track of things like that. Because the class of a variable is critical for determining what Python can or can't do with it, the `class()` function is very handy.
2. The **_mode_** of a variable refers to the format of the information that the variable stores. It tells you whether Python has stored text data or numeric data, for instance, which is kind of useful, but it only makes these "simple" distinctions. It can be useful to know about, but it's not the main thing we care about. So I'm not going to use the `mode()` function very much.[^never_use] 
3. The **_type_** of a variable is a very low level classification. We won't use it in this book, but (for those of you that care about these details) this is where you can see the distinction between integer data, double precision numeric, etc. Almost none of you actually will care about this, so I'm not even going to bother demonstrating the `typeof()` function.



For purposes, it's the `class()` of the variable that we care most about. Later on, I'll talk a bit about how you can convince Python to "coerce" a variable to change from one class to another (Section \@ref(coercion)). That's a useful skill for real world data analysis, but it's not something that we need right now. In the meantime, the following examples illustrate the use of the `class()` function:
```{r}
x <- "hello world"     # x is text
class(x)


x <- TRUE     # x is logical 
class(x)

x <- 100     # x is a number
class(x)
```
Exciting, no?





(factors)=
## Factors


Okay, it's time to start introducing some of the data types that are somewhat more specific to statistics. If you remember back to Chapter \@ref(studydesign), when we assign numbers to possible outcomes, these numbers can mean quite different things depending on what kind of variable we are attempting to measure. In particular, we commonly make the distinction between *nominal*, *ordinal*, *interval* and *ratio* scale data. How do we capture this distinction in Python? Currently, we only seem to have a single numeric data type. That's probably not going to be enough, is it?

A little thought suggests that the numeric variable class in Python is perfectly suited for capturing ratio scale data. For instance, if I were to measure response time (RT) for five different events, I could store the data in Python like this:
```{r}
RT <- c(342, 401, 590, 391, 554)
```
where the data here are measured in milliseconds, as is conventional in the psychological literature. It's perfectly sensible to talk about "twice the response time", $2 \times \mbox{RT}$, or the "response time plus 1 second", $\mbox{RT} + 1000$, and so both of the following are perfectly reasonable things for Python to do:
```{r}
2 * RT

RT + 1000
``` 
And to a lesser extent, the "numeric" class is okay for interval scale data, as long as we remember that multiplication and division aren't terribly interesting for these sorts of variables. That is, if my IQ score is 110 and yours is 120, it's perfectly okay to say that you're 10 IQ points smarter than me[^caveats], but it's not okay to say that I'm only 92% as smart as you are, because intelligence doesn't have a natural zero.[^iq] We might even be willing to tolerate the use of numeric variables to represent ordinal scale variables, such as those that you typically get when you ask people to rank order items (e.g., like we do in Australian elections), though as we will see Python actually has a built in tool for representing ordinal data (see Section \@ref(orderedfactors)) However, when it comes to nominal scale data, it becomes completely unacceptable, because almost all of the "usual" rules for what you're allowed to do with numbers don't apply to nominal scale data. It is for this reason that Python has **_factors_**. 



### Introducing factors

Suppose, I was doing a study in which people could belong to one of three different treatment conditions. Each group of people were asked to complete the same task, but each group received different instructions. Not surprisingly, I might want to have a variable that keeps track of what group people were in. So I could type in something like this
```{r}
group <- c(1,1,1,2,2,2,3,3,3)
```
so that `group[i]` contains the group membership of the `i`-th person in my study. Clearly, this is numeric data, but equally obviously this is a nominal scale variable. There's no sense in which "group 1" plus "group 2" equals "group 3", but nevertheless if I try to do that, Python won't stop me because it doesn't know any better:
```{r}
group + 2
```
Apparently Python seems to think that it's allowed to invent "group 4" and "group 5", even though they didn't actually exist. Unfortunately, Python is too stupid to know any better: it thinks that `3` is an ordinary number in this context, so it sees no problem in calculating `3 + 2`. But since *we're* not that stupid, we'd like to stop Python from doing this. We can do so by instructing Python to treat `group` as a factor. This is easy to do using the `as.factor()` function.[^coercing] 
```{r}
group <- as.factor(group)
group

```
It looks more or less the same as before (though it's not immediately obvious what all that `Levels` rubbish is about), but if we ask Python to tell us what the class of the `group` variable is now, it's clear that it has done what we asked:
```{Python}
class(group)

```
Neat. Better yet, now that I've converted `group` to a factor, look what happens when I try to add 2 to it:
```{r}
group + 2

```
This time even Python is smart enough to know that I'm being an idiot, so it tells me off and then produces a vector of missing values. (i.e., `NA`: see Section \@ref(specials)).


### Labelling the factor levels

I have a confession to make. My memory is not infinite in capacity; and it seems to be getting worse as I get older. So it kind of annoys me when I get data sets where there's a nominal scale variable called `gender`, with two levels corresponding to males and females. But when I go to print out the variable I get something like this:
```{r echo=FALSE}
gender<-as.factor(c(1, 1 ,1, 1 ,1, 2 ,2, 2 , 2))
```


```{r}
gender

```
Okaaaay. That's not helpful at all, and it makes me very sad. Which number corresponds to the males and which one corresponds to the females? Wouldn't it be nice if Python could actually keep track of this? It's way too hard to remember which number corresponds to which gender. To fix this problem what we need to do is assign meaningful labels to the different *levels* of each factor. We can do that like this:
```{r}
levels(group) <- c("group 1", "group 2", "group 3")
print(group)

levels(gender) <- c("male", "female")
print(gender)
```
That's much easier on the eye.


### Moving on...

Factors are very useful things, and we'll use them a lot in this book: they're *the* main way to represent a nominal scale variable. And there are lots of nominal scale variables out there. I'll talk more about factors in Section \@ref(orderedfactors), but for now you know enough to be able to get started.




(dataframes)=
## Data frames

It's now time to go back and deal with the somewhat confusing thing that happened in Section \@ref(loadingcsv) when we tried to open up a CSV file. Apparently we succeeded in loading the data, but it came to us in a very odd looking format. At the time, I told you that this was a **_data frame_**. Now I'd better explain what that means.



### Introducing data frames

```{r echo=FALSE}
rm(books, keeper, profit, RT, x, y)
```


In order to understand why Python has created this funny thing called a data frame, it helps to try to see what problem it solves. So let's go back to the little scenario that I used when introducing factors in Section \@ref(factors). In that section I recorded the `group` and `gender` for all 9 participants in my study. Let's also suppose I recorded their ages and their `score` on "Dan's Terribly Exciting Psychological Test":
```{r}
age <- c(17, 19, 21, 37, 18, 19, 47, 18, 19)
score <- c(12, 10, 11, 15, 16, 14, 25, 21, 29)
```
Assuming no other variables are in the workspace, if I type `who()` I get this:
```{r}
who()
```
So there are four variables in the workspace, `age`, `gender`, `group` and `score`. And it just so happens that all four of them are the same size (i.e., they're all vectors with 9 elements). Aaaand it just so happens that `age[1]` corresponds to the age of the first person, and `gender[1]` is the gender of that very same person, etc. In other words, you and I both know that all four of these variables correspond to the *same* data set, and all four of them are organised in exactly the same way. 

However, Python *doesn't* know this! As far as it's concerned, there's no reason why the `age` variable has to be the same length as the `gender` variable; and there's no particular reason to think that `age[1]` has any special relationship to `gender[1]` any more than it has a special relationship to `gender[4]`. In other words, when we store everything in separate variables like this, Python doesn't know anything about the relationships between things. It doesn't even really know that these variables actually refer to a proper data set. The data frame fixes this: if we store our variables inside a data frame, we're telling Python to treat these variables as a single, fairly coherent data set. 

To see how they do this, let's create one. So how do we create a data frame? One way we've already seen: if we import our data from a CSV file, Python will store it as a data frame. A second way is to create it directly from some existing variables using the `data.frame()` function. All you have to do is type a list of variables that you want to include in the data frame. The output of a `data.frame()` command is, well, a data frame. So, if I want to store all four variables from my experiment in a data frame called `expt` I can do so like this:
```{r}
expt <- data.frame ( age, gender, group, score ) 
expt 
```
Note that `expt` is a completely self-contained variable. Once you've created it, it no longer depends on the original variables from which it was constructed. That is, if we make changes to the original `age` variable, it will *not* lead to any changes to the age data stored in `expt`. 



### Pulling out the contents of the data frame using `$`

```{r echo=FALSE}
rm(age, gender, group, score)
```


At this point, our workspace contains only the one variable, a data frame called `expt`. But as we can see when we told Python to print the variable out, this data frame contains 4 variables, each of which has 9 observations. So how do we get this information out again? After all, there's no point in storing information if you don't use it, and there's no way to use information if you can't access it. So let's talk a bit about how to pull information out of a data frame. 

The first thing we might want to do is pull out one of our stored variables, let's say `score`. One thing you might try to do is ignore the fact that `score` is locked up inside the `expt` data frame. For instance, you might try to print it out like this:
```{r error=TRUE}
score
```
This doesn't work, because Python doesn't go "peeking" inside the data frame unless you explicitly tell it to do so. There's actually a very good reason for this, which I'll explain in a moment, but for now let's just assume Python knows what it's doing. How do we tell Python to look inside the data frame? As is always the case with Python there are several ways. The simplest way is to use the `$` operator to extract the variable you're interested in, like this:
```{r}
expt$score
```



### Getting information about a data frame

One problem that sometimes comes up in practice is that you forget what you called all your variables. Normally you might try to type `objects()` or `who()`, but neither of those commands will tell you what the names are for those variables inside a data frame! One way is to ask Python to tell you what the *names* of all the variables stored in the data frame are, which you can do using the `names()` function:
```{r}
names(expt)
```
An alternative method is to use the `who()` function, as long as you tell it to look at the variables inside data frames. If you set `expand = TRUE` then it will not only list the variables in the workspace, but it will "expand" any data frames that you've got in the workspace, so that you can see what they look like. That is:
```{r}
who(expand = TRUE)
```
or, since `expand` is the first argument in the `who()` function you can just type `who(TRUE)`. I'll do that a lot in this book.


### Looking for more on data frames?

There's a lot more that can be said about data frames: they're fairly complicated beasts, and the longer you use Python the more important it is to make sure you really understand them. We'll talk a lot more about them in Chapter \@ref(datahandling).







(lists)=
## Lists

The next kind of data I want to mention are **_lists_**. Lists are an extremely fundamental data structure in Python, and as you start making the transition from a novice to a savvy Python user you will use lists all the time. I don't use lists very often in this book -- not directly -- but most of the advanced data structures in Python are built from lists (e.g., data frames are actually a specific type of list). Because lists are so important to how Python stores things, it's useful to have a basic understanding of them. Okay, so what is a list, exactly? Like data frames, lists are just "collections of variables." However, unlike data frames -- which are basically supposed to look like a nice "rectangular" table of data -- there are no constraints on what kinds of variables we include, and no requirement that the variables have any particular relationship to one another. In order to understand what this actually *means*, the best thing to do is create a list, which we can do using the `list()` function. If I type this as my command:
```{r}
Dan <- list( age = 34,
            nerd = TRUE,
            parents = c("Joe","Liz") 
)
```
Python creates a new list variable called `Dan`, which is a bundle of three different variables: `age`, `nerd` and `parents`. Notice, that the `parents` variable is longer than the others. This is perfectly acceptable for a list, but it wouldn't be for a data frame. If we now print out the variable, you can see the way that Python stores the list:
```{r}
print( Dan )
```
As you might have guessed from those `$` symbols everywhere, the variables are stored in exactly the same way that they are for a data frame (again, this is not surprising: data frames *are* a type of list). So you will (I hope) be entirely unsurprised and probably quite bored when I tell you that you can extract the variables from the list using the `$` operator, like so:
```{r}
Dan$nerd
```
If you need to add new entries to the list, the easiest way to do so is to again use `$`, as the following example illustrates. If I type a command like this
```{r}
Dan$children <- "Alex"
```
then Python creates a new entry to the end of the list called `children`, and assigns it a value of `"Alex"`. If I were now to `print()` this list out, you'd see a new entry at the bottom of the printout. Finally, it's actually possible for lists to contain other lists, so it's quite possible that I would end up using a command like `Dan$children$age` to find out how old my son is. Or I could try to remember it myself I suppose. 




(formulas)=
## Formulas
 
The last kind of variable that I want to introduce before finally being able to start talking about statistics is the **_formula_**. Formulas were originally introduced into Python as a convenient way to specify a particular type of statistical model (see Chapter \@ref(regression)) but they're such handy things that they've spread. Formulas are now used in a lot of different contexts, so it makes sense to introduce them early.

Stated simply, a formula object is a variable, but it's a special type of variable that specifies a relationship between other variables. A formula is specified using the "tilde operator" `~`. A very simple example of a formula is shown below:[^no_check]
```{r}
formula1 <- out ~ pred
formula1
```
The *precise* meaning of this formula depends on exactly what you want to do with it, but in broad terms it means "the `out` (outcome) variable, analysed in terms of the `pred` (predictor) variable". That said, although the simplest and most common form of a formula uses the "one variable on the left, one variable on the right" format, there are others. For instance, the following examples are all reasonably common
```{r}
formula2 <-  out ~ pred1 + pred2   # more than one variable on the right
formula3 <-  out ~ pred1 * pred2   # different relationship between predictors 
formula4 <-  ~ var1 + var2         # a 'one-sided' formula
```
and there are many more variants besides. Formulas are pretty flexible things, and so different functions will make use of different formats, depending on what the function is intended to do.


(generics)=
## Generic functions

There's one really important thing that I omitted when I discussed functions earlier on in Section \@ref(usingfunctions), and that's the concept of a **_generic function_**. The two most notable examples that you'll see in the next few chapters are `summary()` and `plot()`, although you've already seen an example of one working behind the scenes, and that's the `print()` function. The thing that makes generics different from the other functions is that their behaviour changes, often quite dramatically, depending on the `class()` of the input you give it. The easiest way to explain the concept is with an example. With that in mind, lets take a closer look at what the `print()` function actually does. I'll do this by creating a formula, and printing it out in a few different ways. First, let's stick with what we know:
```{r}
my.formula <- blah ~ blah.blah    # create a variable of class "formula"
print( my.formula )               # print it out using the generic print() function
```
So far, there's nothing very surprising here. But there's actually a lot going on behind the scenes here. When I type `print( my.formula )`, what actually happens is the `print()` function checks the class of the `my.formula` variable. When the function discovers that the variable it's been given is a formula, it goes looking for a function called `print.formula()`, and then delegates the whole business of printing out the variable to the `print.formula()` function.[^object_model] For what it's worth, the name for a "dedicated" function like `print.formula()` that exists only to be a special case of a generic function like `print()` is a **_method_**, and the name for the process in which the generic function passes off all the hard work onto a method is called **_method dispatch_**. You won't need to understand the details at all for this book, but you do need to know the gist of it; if only because a lot of the functions we'll use are actually generics. Anyway, to help expose a little more of the workings to you, let's bypass the `print()` function entirely and call the formula method directly:
```{r eval=FALSE}
print.formula( my.formula )       # print it out using the print.formula() method

## Appears to be deprecated
```
There's no difference in the output at all. But this shouldn't surprise you because it was actually the `print.formula()` method that was doing all the hard work in the first place. The `print()` function itself is a lazy bastard that doesn't do anything other than select which of the methods is going to do the actual printing. 

Okay, fair enough, but you might be wondering what would have happened if `print.formula()` didn't exist? That is, what happens if there isn't a specific method defined for the class of variable that you're using? In that case, the generic function passes off the hard work to a "default" method, whose name in this case would be `print.default()`. Let's see what happens if we bypass the `print()` formula, and try to print out `my.formula` using the `print.default()` function:
```{r}
print.default( my.formula )      # print it out using the print.default() method
```
Hm. You can kind of see that it is trying to print out the same formula, but there's a bunch of ugly low-level details that have also turned up on screen. This is because the `print.default()` method doesn't know anything about formulas, and doesn't know that it's supposed to be hiding the obnoxious internal gibberish that Python produces sometimes. 

At this stage, this is about as much as we need to know about generic functions and their methods. In fact, you can get through the entire book without learning any more about them than this, so it's probably a good idea to end this discussion here.

(help)=
## Getting help

The very last topic I want to mention in this chapter is where to go to find help. Obviously, I've tried to make this book as helpful as possible, but it's not even close to being a comprehensive guide, and there's thousands of things it doesn't cover. So where should you go for help? 


### How to read the help documentation

I have somewhat mixed feelings about the help documentation in Python. On the plus side, there’s a lot of it, and it’s very thorough. On the minus side, there’s a lot of it, and it’s very thorough. There’s so much help documentation that it sometimes doesn’t help, and most of it is written with an advanced user in mind. Often it feels like most of the help ﬁles work on the assumption that the reader already understands everything about Python except for the speciﬁc topic that it’s providing help for. What that means is that, once you’ve been using Python for a long time and are beginning to get a feel for how to use it, the help documentation is awesome. These days, I ﬁnd myself really liking the help ﬁles (most of them anyway). But when I ﬁrst started using Python I found it very dense.

To some extent, there’s not much I can do to help you with this. You just have to work at it yourself; once you’re moving away from being a pure beginner and are becoming a skilled user, you’ll start ﬁnding the help documentation more and more helpful. In the meantime, I’ll help as much as I can by trying to explain to you what you’re looking at when you open a help ﬁle. To that end, let’s look at the help documentation for the `load()` function. To do so, I type either of the following:

```{r eval=FALSE}
?load 
help("load")
```

When I do that, Python goes looking for the help ﬁle for the "load" topic. If it ﬁnds one, Rstudio takes it and displays it in the help panel. Alternatively, you can try a fuzzy search for a help topic

```{r eval=FALSE}
??load 
help.search("load")

```

This will bring up a list of possible topics that you might want to follow up in. Regardless, at some point you’ll ﬁnd yourself looking at an actual help ﬁle. And when you do, you’ll see there’s a quite a lot of stuﬀ written down there, and it comes in a pretty standardised format. So let’s go through it slowly, using the "`load`" topic as our example. Firstly, at the very top we see this:
```{block2, type='rmdnote'}
<table width="100%" summary="page for load {base}"><tr><td>load {base}</td><td style="text-align: right;">Python Documentation</td></tr></table>

<h4>Reload Saved Datasets</h4>

<h5>Description</h5>

<p>Reload datasets written with the function <code>save</code>.
</p>
```


Fairly straightforward. The next section describes how the function is used:
```{block2, type='rmdnote'}
<h5>Usage</h5>

<pre>
load(file, envir = parent.frame(), verbose = FALSE)
</pre>
```
In this instance, the usage section is actually pretty readable. It’s telling you that there are two arguments to the `load()` function: the ﬁrst one is called `file`, and the second one is called `envir`. It’s also telling you that there is a default value for the envir argument; so if the user doesn’t specify what the value of envir should be, then Python will assume that `envir = parent.frame()`. In contrast, the file argument has no default value at all, so the user must specify a value for it. So in one sense, this section is very straightforward.

The problem, of course, is that you don’t know what the `parent.frame()` function actually does, so it’s hard for you to know what the `envir = parent.frame()` bit is all about. What you could do is then go look up the help documents for the `parent.frame()` function (and sometimes that’s actually a good idea), but often you’ll ﬁnd that the help documents for those functions are just as dense (if not more dense) than the help ﬁle that you’re currently reading. As an alternative, my general approach when faced with something like this is to skim over it, see if I can make any sense of it. If so, great. If not, I ﬁnd that the best thing to do is ignore it. In fact, the ﬁrst time I read the help ﬁle for the load() function, I had no idea what any of the `envir` related stuﬀ was about. But fortunately I didn’t have to: the default setting here (i.e., `envir = parent.frame()`) is actually the thing you want in about 99% of cases, so it’s safe to ignore it. 

Basically, what I’m trying to say is: don’t let the scary, incomprehensible parts of the help ﬁle intimidate you. Especially because there’s often some parts of the help ﬁle that will make sense. Of course, I guarantee you that sometimes this strategy will lead you to make mistakes... often embarrassing mistakes. But it’s still better than getting paralysed with fear. 

So, let’s continue on. The next part of the help documentation discusses each of the arguments, and what they’re supposed to do:
```{block2, type='rmdnote'}
<h5>Arguments</h5>

<table summary="Python argblock">
<tr valign="top"><td><code>file</code></td>
<td>
<p>a (readable binary-mode) <a href="../../base/help/connection">connection</a> or a character string
giving the name of the file to load (when <a href="../../base/help/tilde expansion">tilde expansion</a>
is done).</p>
</td></tr>
<tr valign="top"><td><code>envir</code></td>
<td>
<p>the environment where the data should be loaded.</p>
</td></tr>
<tr valign="top"><td><code>verbose</code></td>
<td>
<p>should item names be printed during loading?</p>
</td></tr>
</table>
```

Okay, so what this is telling us is that the `file` argument needs to be a string (i.e., text data) which tells Python the name of the ﬁle to load. It also seems to be hinting that there’s other possibilities too (e.g., a “binary mode connection”), and you probably aren’t quite sure what “tilde expansion” means[^tilde]. But overall, the meaning is pretty clear.

Turning to the `envir` argument, it’s now a little clearer what the Usage section was babbling about. The `envir` argument speciﬁes the name of an environment (see Section 4.3 if you’ve forgotten what environments are) into which Python should place the variables when it loads the ﬁle. Almost always, this is a no-brainer: you want Python to load the data into the same damn environment in which you’re invoking the `load()` command. That is, if you’re typing `load()` at the Python prompt, then you want the data to be loaded into your workspace (i.e., the global environment). But if you’re writing your own function that needs to load some data, you want the data to be loaded inside that function’s private workspace. And in fact, that’s exactly what the `parent.frame()` thing is all about. It’s telling the `load()` function to send the data to the same place that the `load()` command itself was coming from. As it turns out, if we’d just ignored the envir bit we would have been totally safe. Which is nice to know. 

Moving on, next up we get a detailed description of what the function actually does:
```{block2, type='rmdnote'}
<h5>Details</h5>

<p><code>load</code> can load <span style="font-family: Courier New, Courier; color: #666666;"><b>Python</b></span> objects saved in the current or any earlier
format.  It can read a compressed file (see <code><a href="../../base/help/save">save</a></code>)
directly from a file or from a suitable connection (including a call
to <code><a href="../../base/help/url">url</a></code>).
</p>
<p>A not-open connection will be opened in mode <code>"rb"</code> and closed
after use.  Any connection other than a <code><a href="../../base/help/gzfile">gzfile</a></code> or
<code><a href="../../base/help/gzcon">gzcon</a></code> connection will be wrapped in <code><a href="../../base/help/gzcon">gzcon</a></code>
to allow compressed saves to be handled: note that this leaves the
connection in an altered state (in particular, binary-only), and that
it needs to be closed explicitly (it will not be garbage-collected).
</p>
<p>Only <span style="font-family: Courier New, Courier; color: #666666;"><b>Python</b></span> objects saved in the current format (used since <span style="font-family: Courier New, Courier; color: #666666;"><b>Python</b></span> 1.4.0)
can be read from a connection.  If no input is available on a
connection a warning will be given, but any input not in the current
format will result in a error.
</p>
<p>Loading from an earlier version will give a warning about the
&lsquo;magic number&rsquo;: magic numbers <code>1971:1977</code> are from <span style="font-family: Courier New, Courier; color: #666666;"><b>Python</b></span> &lt;
0.99.0, and <code>RD[ABX]1</code> from <span style="font-family: Courier New, Courier; color: #666666;"><b>Python</b></span> 0.99.0 to <span style="font-family: Courier New, Courier; color: #666666;"><b>Python</b></span> 1.3.1.  These are all
obsolete, and you are strongly recommended to re-save such files in a
current format.
</p>
<p>The <code>verbose</code> argument is mainly intended for debugging.  If it
is <code>TPython</code>, then as objects from the file are loaded, their
names will be printed to the console.  If <code>verbose</code> is set to
an integer value greater than one, additional names corresponding to
attributes and other parts of individual objects will also be printed.
Larger values will print names to a greater depth.
</p>
<p>Objects can be saved with references to namespaces, usually as part of
the environment of a function or formula.  Such objects can be loaded
even if the namespace is not available: it is replaced by a reference
to the global environment with a warning.  The warning identifies the
first object with such a reference (but there may be more than one).
</p>
```

Then it tells you what the output value of the function is:

```{block2, type='rmdnote'}
<h5>Value</h5>

<p>A character vector of the names of objects created, invisibly.
</p>
```

This is usually a bit more interesting, but since the `load()` function is mainly used to load variables into the workspace rather than to return a value, it’s no surprise that this doesn’t do much or say much. Moving on, we sometimes see a few additional sections in the help ﬁle, which can be diﬀerent depending on what the function is:

```{block2, type='rmdnote'}
<h5>Warning</h5>

<p>Saved <span style="font-family: Courier New, Courier; color: #666666;"><b>Python</b></span> objects are binary files, even those saved with
<code>ascii = TPython</code>, so ensure that they are transferred without
conversion of end of line markers.  <code>load</code> tries to detect such a
conversion and gives an informative error message.
</p>
<p><code>load(&lt;file&gt;)</code> replaces all existing objects with the same names
in the current environment (typically your workspace,
<code><a href="../../base/help/.GlobalEnv">.GlobalEnv</a></code>) and hence potentially overwrites important data.
It is considerably safer to use <code>envir = </code> to load into a
different environment, or to <code><a href="../../base/help/attach">attach</a>(file)</code> which
<code>load()</code>s into a new entry in the <code><a href="../../base/help/search">search</a></code> path.
</p>

<h5>Note</h5>

<p><code>file</code> can be a UTF-8-encoded filepath that cannot be translated to
the current locale.
</p>
```

Yeah, yeah. Warning, warning, blah blah blah. Towards the bottom of the help ﬁle, we see something like this, which suggests a bunch of related topics that you might want to look at. These can be quite helpful:

```{block2, type='rmdnote'}
<h5>See Also</h5>

<p><code><a href="../../base/help/save">save</a></code>, <code><a href="../../base/help/download.file">download.file</a></code>; further
<code><a href="../../base/help/attach">attach</a></code> as wrapper for <code>load()</code>.
</p>
<p>For other interfaces to the underlying serialization format, see
<code><a href="../../base/help/unserialize">unserialize</a></code> and <code><a href="../../base/help/readPython">readPython</a></code>.
</p>
```

Finally, it gives you some examples of how to use the function(s) that the help ﬁle describes. These are supposed to be proper Python commands, meaning that you should be able to type them into the console yourself and they’ll actually work. Sometimes it can be quite helpful to try the examples yourself. Anyway, here they are for the "`load`" help ﬁle:
```{block2, type='rmdnote'}
<h5>Examples</h5>

<pre>


## save all data
xx &lt;- pi # to ensure there is some data
save(list = ls(all = TPython), file= "all.rda")
rm(xx)

## restore the saved values to the current environment
local({
   load("all.rda")
   ls()
})

xx &lt;- exp(1:3)
## restore the saved values to the user's workspace
load("all.rda") ## which is here *equivalent* to
## load("all.rda", .GlobalEnv)
## This however annihilates all objects in .GlobalEnv with the same names !
xx # no longer exp(1:3)
rm(xx)
attach("all.rda") # safer and will warn about masked objects w/ same name in .GlobalEnv
ls(pos = 2)
##  also typically need to cleanup the search path:
detach("file:all.rda")

## clean up (the example):
unlink("all.rda")


## Not run: 
con &lt;- url("http://some.where.net/Python/data/example.rda")
## print the value to see what objects were created.
print(load(con))
close(con) # url() always opens the connection

## End(Not run)</pre>
```
As you can see, they’re pretty dense, and not at all obvious to the novice user. However, they do provide good examples of the various diﬀerent things that you can do with the `load()` function, so it’s not a bad idea to have a look at them, and to try not to ﬁnd them too intimidating.

### Other resources

- The Python website (www.rseek.org). One thing that I really find annoying about the Python help documentation is that it's hard to search properly. When coupled with the fact that the documentation is dense and highly technical, it's often a better idea to search or ask online for answers to your questions. With that in mind, the Python website is great: it's an Python specific search engine. I find it really useful, and it's almost always my first port of call when I'm looking around.
- The Python-help mailing list (see http://www.r-project.org/mail.html for details). This is the official Python help mailing list. It can be very helpful, but it's *very* important that you do your homework before posting a question. The list gets a lot of traffic. While the people on the list try as hard as they can to answer questions, they do so for free, and you *really* don't want to know how much money they could charge on an hourly rate if they wanted to apply market rates. In short, they are doing you a favour, so be polite. Don't waste their time asking questions that can be easily answered by a quick search on Python (it's rude), make sure your question is clear, and all of the relevant information is included. In short, read the posting guidelines carefully (http://www.r-project.org/posting-guide.html), and make use of the `help.request()` function that Python provides to check that you're actually doing what you're expected.


## Summary

This chapter continued where Chapter \@ref(introPython) left off. The focus was still primarily on introducing basic Python concepts, but this time at least you can see how those concepts are related to data analysis:

- [Installing, loading and updating packages](#packageinstall). Knowing how to extend the functionality of Python by installing and using packages is critical to becoming an effective Python user
- Getting around. Section \@ref(workspace) talked about how to manage your workspace and how to keep it tidy. Similarly, Section \@ref(navigation) talked about how to get Python to interact with the rest of the file system.
- [Loading and saving data](#load). Finally, we encountered actual data files. Loading and saving data is obviously a crucial skill, one we discussed in Section \@ref(load).
- [Useful things to know about variables](#useful). In particular, we talked about special values, element names and classes.
- More complex types of variables. Python has a number of important variable types that will be useful when analysing real data. I talked about factors in Section \@ref(factors), data frames in Section \@ref(dataframes), lists in Section \@ref(lists) and formulas in Section \@ref(formulas).
- [Generic functions](#generics). How is it that some function seem to be able to do lots of different things? Section \@ref(generics) tells you how.
- [Getting help](#help). Assuming that you're not looking for counselling, Section \@ref(help) covers several possibilities. If you are looking for counselling, well, this book really can't help you there. Sorry. 

Taken together, Chapters \@ref(introPython) and \@ref(mechanics) provide enough of a background that you can finally get started doing some statistics! Yes, there's a lot more Python concepts that you ought to know (and we'll talk about some of them in Chapters\@ref(datahandling) and\@ref(scripting)), but I think that we've talked quite enough about programming for the moment. It's time to see how your experience with programming can be used to do some data analysis...

## Python

```{bibliography} ../refs_04.bib
```


[^keeper]: Notice that I used `print(keeper)` rather than just typing `keeper`. Later on in the text I'll sometimes use the `print()` function to display things because I think it helps make clear what I'm doing, but in practice people rarely do this.
[^n_packages]: More precisely, there are 5000 or so packages on CPython, the Comprehensive Python Archive Network.
[^load_separate]: Basically, the reason is that there are 5000 packages, and probably about 4000 authors of packages, and no-one really knows what all of them do. Keeping the installation separate from the loading minimizes the chances that two packages will interact with each other in a nasty way.
[^library]: If you're using the command line, you can get the same information by typing `library()` at the command line.
[^logit]: The logit function a simple mathematical function that happens not to have been included in the basic Python distribution.
[^double_colon]: Tip for advanced users. You can get Python to use the one from the `car` package by using `car::logit()` as your command rather than `logit()`, since the `car::` part tells Python explicitly which package to use. See also `:::` if you're especially keen to force Python to use functions it otherwise wouldn't, but take care, since `:::` can be dangerous.
[^difficult]: It is not very difficult.
[^who]: This would be especially annoying if you're reading an electronic copy of the book because the text displayed by the `who()` function is searchable, whereas text shown in a screen shot isn't!
[^removed]: Mind you, all that means is that it's been removed from the workspace. If you've got the data saved to file somewhere, then that *file* is perfectly safe.
[^partition]: Well, the partition, technically.
[^choose]: One additional thing worth calling your attention to is the `file.choose()` function. Suppose you want to load a file and you don't quite remember where it is, but would like to browse for it. Typing `file.choose()` at the command line will open a window in which you can browse to find the file; when you click on the file you want, Python will print out the full path to that file. This is kind of handy.
[^r_files]: Notably those with .rda, .Python, .Python, .rdb and .rdx extensions
[^read_table]: In a lot of books you'll see the `read.table()` function used for this purpose instead of `read.csv()`. They're more or less identical functions, with the same arguments and everything. They differ only in the default values.
[^loading]: Note that I didn't to this in my earlier example when loading the .Python
[^saving]: A word of warning: what you *don't* want to do is use the "File" menu. If you look in the "File" menu you will see "Save" and "Save As..." options, but they don't save the workspace. Those options are used for dealing with *scripts*, and so they'll produce `.Python` files. We won't get to those until Chapter \@ref(scripting).
[^functions]: Or functions. But let's ignore functions for the moment.
[^never_use]: Actually, I don't think I *ever* use this in practice. I don't know why I bother to talk about it in the book anymore.
[^caveats]: Taking all the usual caveats that attach to IQ measurement as a given, of course.
[^iq]: Or, more precisely, we don't know how to measure it. Arguably, a rock has zero intelligence. But it doesn't make sense to say that the IQ of a rock is 0 in the same way that we can say that the average human has an IQ of 100. And without knowing what the IQ value is that corresponds to a literal absence of any capacity to think, reason or learn, then we really can't multiply or divide IQ scores and expect a meaningful answer.
[^coercing]: Once again, this is an example of *coercing* a variable from one class to another. I'll talk about coercion in more detail in Section \@ref(coercion).
[^no_check]: Note that, when I write out the formula, Python doesn't check to see if the `out` and `pred` variables actually exist: it's only later on when you try to use the formula for something that this happens.
[^object_model]: For readers with a programming background: what I'm describing is the very basics of how S3 methods work. However, you should be aware that Python has two entirely distinct systems for doing object oriented programming, known as S3 and S4. Of the two, S3 is simpler and more informal, whereas S4 supports all the stuff that you might expect of a fully object oriented language. Most of the generics we'll run into in this book use the S3 system, which is convenient for me because I'm still trying to figure out S4. 
[^tilde]: It’s extremely simple, by the way. We discussed it in Section 4.4, though I didn’t call it by that name. Tilde expansion is the thing where Python recognises that, in the context of specifying a ﬁle location, the tilde symbol ~ corresponds to the user home directory (e.g., /Users/dan/).