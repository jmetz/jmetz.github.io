---
layout: default
---
# Python 2016: Advanced Python & Data Analysis

[![XKCDSW comic](https://imgs.xkcd.com/comics/python.png)](https://xkcd.com/353/)

## Introduction

These posts are intended as an introduction and guide
to using advanced Python for data analysis and research. 

As big a part of this series of posts as any of the formal aims listed below, 
is that you should enjoy yourself. Python is a fun language because it is
relatively easy to read and tells you all about what you did wrong (or what 
module was broken) if an error occurs.

With that in mind, have fun, and happy learning!
## Recap: What is Python?

Python is the name of a programming language (created by Dutch 
programmer [Guido Van Rossum](https://en.wikipedia.org/wiki/Guido_van_Rossum)
as a hobby programming project!), as well as the program, known as 
an interpreter, that executes scripts (text files) written in that language. 

Van Rossum named his new programming language after Monty Python's Flying Circus 
(he was reading the published scripts from "Monty Pythonâ€™s Flying Circus" at the 
time of developing Python!). 

It is common to use Monty Python references in example code. 
For example, the dummy (aka metasyntactic) variables often used in Python 
literature are spam and eggs, instead of the traditional foo and bar.
As well as this, the official Python documentation often contains various 
obscure Monty Python references.


> **Jargon**
> 
> The program is known as an interpreter because it interprets 
> human readable code into computer readable code and executes it. 
> This is in contrast to *compiled* programming languages like C, C++, 
> and Java which split this process up into a compile step (conversion of 
> human-readable code into computer code) and a separate execution step, which 
> is what happens when you press on a typical program executable, or run a 
> Java class file using the Java Virtual Machine. 

Because of it's focus on code readability and highly expressive syntax 
(meaning that programmers can write less code than would be required with 
languages like C or Java), Python has grown hugely in popularity and 
is now one of the most popular programming languages in use. 


## Aims

* Advanced Python concepts like 
    * Comprehensions (e.g. `[ abs(x) for x in y ]`)
    * Context managers (`with`)
    * Generators (`yield`)
* Numerical analysis with Numpy 
* Visualizing data with Matplotlib
* Scipy: Additional modules 


### Installing on your own machine


If you want to use Python on your own computer I would recomend using
one of the following "distributions" of Python, rather than just the 
basic Python interpreter. 

Amongst other things, these distributions take the pain out of getting 
started because they include all modules you're likely to need to get started
as well as links to pre-configured consoles that make running Python a breeze.  

* [Anaconda (Win, MacOS, Linux)](https://www.continuum.io/downloads) : Commercially backed free distribution
* [WinPython (Windows Only)](https://winpython.github.io/) : Open-source free distribution
* Linux : Python 2 is preinstalled on most linux distributions; to install Python 3, simply use your favourite package manager. E.g. on debian based systems (Debian, Ubuntu, Mint), running `sudo apt-get install python3` from a terminal will install Python 3. Alternatively use Anaconda.

**Note** : Be sure to download the Python **3**, not 2, and get the correct acrchitecture for your machine (ie 32 or 64 bit). 



# Advanced Python 

You should already be familiar with basic Python including 

* Basic data types (numbers, strings)
* Container types like lists and dicts
* Controlling program flow with `if`-`else`, `for`, etc
* Reading and writing files
* Defining functions
* Documenting code 
* Using modules

If you are unclear on any of these points, please refer back through the 
introductory notes. 

We will now focus on some advanced, sometimes Python-specific, concepts. 
By this I mean that even if you know how to program in C or Fortran, 
you will possiby not know for example what a list comprehension is!

Less "universally useful" advanced Python concepts have been included in the 
_optional_ "Additional Advanced Python" section. 

## Easier to ask for forgiveness than permission: `try`ing

A common programming approach in Python (as opposed to e.g. C) is 
to **try** something and catch the exception (i.e. if it fails).

To do this we use a `try`-`except` block, for example
```py
# At this point we have a variable called my_var that contains a string
try: 
    num = float(my_var)
except:
    # Calling float on my_var failed, it must not be a 
    # string representation of a number!
    num = 0
```
i.e. instead of carefully inspecting the string contained my_var to determine
whether we _can_ convert it to a number, the Pythonic approach is to simply
`try` and convert it to a number and handle the case that that fails in 
an `except` block.
## Common Standard Library modules: `os` and `sys`

We've already encountered `os` and `sys` in the introductory notes. 
However, there are some common uses of `os` and `sys` functions that
merit special mention. 

### File-name operations: `os.path`

When, generating full file-paths, or extracting file directory locations,
we could potentially use simple string manipulation functions. 

For example, if we want to determine the folder of the path string 
`"C:\Some\Directory\Path\my_favourite_file.txt"` 
we might think to split the string at the backslash character "\" and
return all but the last element of the resulting list (and then 
put them together again). However, not only is this tedious, but it is 
also then platform dependent (Linux and MacOS use forward-slashes instead 
of back-slashes). 

Instead, it is much better to use the `os.path` submodule to 
* create paths from folder names using `os.path.join`
* determine a (full) filepath's directory using `os.path.dirname`
* Split a file at the extension (for generating e.g. output files) using `os.path.splitext`
amongst many more useful file path manipulation functions. 

### Getting command-line input using `sys.argv`

`sys.argv` is a list of command line arguments (starting with the 
script file name) - you can access it's elements to get command line 
inputs. 

To test and play with this you can simply add the lines
```py 
import sys
print(sys.argv)
```
to the top of any (or an empty) script, and run the script. 
If you follow the script file name by (a space and then) additional words, 
you will see these words appear in the terminal output as being contained in 
`sys.argv`. 

## String formatting

Another common Python task is creating strings with formatted representations of 
numbers.

You should already know that the `print` function is good at printing out a variety of 
data types for us. 
Internally, it creates string representations of non-string data before printint the final 
strings to the terminal. 

To control that process, we often perform what is know as string formatting. 
To create a format string use the following special symbols in the string
* `"%s"` will be substituted by a string
* `"%d"` will be substituted by the integer representation of a number
* `"%f"` will be substituted by the float representation of a number 

We can also specify additional formatting contraints in these special codes. For example to 
create a fixed length integer representation of a number we might use 
```py
print( "%.8d"%99 )
```
which outputs `00000099` to the terminal; i.e. the `.8` part of the code meant : "always make the 
number 8 characters long, (appending zeros as necessary). 

**NOTE:** The new way of doing this is to use the `format` member function; 
```py
print("{:08d}".format(99))
```
though the old way also still works!

For additional format string options in the context of the newer `format` string method, see the 
documentation [here](https://docs.python.org/3/library/string.html#format-string-syntax). 

## Comprehensions

Comprehensions are losely speaking shorthand ways to quickly generate
lists, dictionaries, or generators (see later) from relatively simple
expressions. 

Consider a `for` loop used to generate a list that holds the 
squares of another list: 

```py
list1 = [10, 20, 30, 40]
list2 = []
for val in list1:
    list2.append( val * val ) # or equivalently val ** 2
```
The last 3 lines can be neatly replaced using a **list comprehension**:
```py
list1 = [10, 20, 30, 40]
list2 = [val*val for val in list1]
```

That's it! Simple, clean, and easy to understand once you know how. 

In words what this means is: _"set `list2` to : a new list, where the list 
items are equal to val*val where val is equal to each item in list `list1`"_.
`list2` will then be equal to `[100, 400, 900, 1600]`.
The list comprehension can work with any type of item, e.g. 
```py
list3 = ["spam", "and", "eggs"]
list4 = [ thing.capitalize() for thing in list3 ]
```
would set `list4` equal to `["Spam", "And", "Eggs"]`. 


Similarly you can generate a dictionary (nb. dictionaries are created with braces, aka curly brackets) comprehension 
```py
words = ['the', 'fast', 'brown', 'fox']
lengths = {word : len(word) for word in words }
```
(this generates the dictionary 
`{'the':3, 'fast':4, 'brown':5, 'fox':3}` assigned to `lengths`)

The last example is using **tuple** syntax (re; tuples are defined using parentheses (aka round brackets), 
```py
list1 = [10, 20, 30, 40]
gen   = ( val * val for val in list1)
```
but the crucial difference is that gen is **not** a tuple (nor a list). 
It is a generator object, which we will learn about below. 

### Adding logic to comprehensions

Comprehensions can also include simple logic, to decide which elements 
to include. 
For example, if we have a list of files, we might want to filter the files 
to only include files that end in a specific extension. This could be done
by adding an `if` section to the end of the comprehension; 

```py
# e.g. file list 
file_list = ["file1.txt", "file2.py", "file3.tif", "file4.txt"]
text_files = [f for f in file_list if f.endswith("txt")]
```
This code would result in the variable `text_files` holding the list 
`["file1.txt", "file4.txt"]` - i.e. only the strings that ended in "txt"!


## Context managers : `with`

A context manager is a construct to allocate a resource when you need it 
and handle any required cleanup operations when you're done. 

One of the most common examples of where context managers are useful in 
Python is reading/writting files.

Instead of 
```py
fid = open('filename.txt')
for line in fid:
    print(line)
fid.close()    
```
we can write
```py
with open('filename.txt') as fid:
    for line in fid:
        print(line)
```
Not only do we save a line of code, but we also avoid forgetting to close 
the file and potentially running into errors if we were to try and open 
the file again later in the code. 

Context managers are also used in situations such as in the threading module
to lock a thread, and can in fact be added to any function or class
using `contextlib`. 

If you're interested in advanced uses of context managers, see e.g. 

* [http://eigenhombre.com/2013/04/20/introduction-to-context-managers/](http://eigenhombre.com/2013/04/20/introduction-to-context-managers/)
* [http://preshing.com/20110920/the-python-with-statement-by-example/](http://preshing.com/20110920/the-python-with-statement-by-example/)
* [https://www.python.org/dev/peps/pep-0343/](https://www.python.org/dev/peps/pep-0343/)

Otherwise, other than switching to using `with` when opening files, 
you probably won't encouter them too often!


## Useful built-in global variables : `__file__`, `__name__` 

I sneakily introduced some **built-in** _global_ variables during the 
Introdcutory course - apologies to those of you who wondered what they were and 
where they came from!

The reason I used these variables (namely `__file__` and `__name__`), 
was to make things easier with respect to accessing the current script's
file name, and creating a section of code that would only run if the script
was being run as a script (i.e. not being imported to another script), 
respectively.

Firstly a note about naming; the built-in variables are named using two 
leading and trailing underscores (`__`), which in Python is the convention 
for letting other developers know that they shouldn't change a variable. 

This is because other modules and functions will likely also make use of these
variables, so if you change their values, you might break these other modules 
and functions!

Now that these built-in variables have been introduced and their naming 
explained, which built-in variables are available, and what are they useful for? 

* `__file__` : holds the name of the currently executing script
* `__name__` : holds the name of the current module (or the value 
  `"__main__"` if the script is run as a script - making it useful for 
  adding a script-running-only section of code)
* `__doc__` : Holds the current module's (/function/object) docstring
* `__package__` : Holds the name of the package the current module belongs to

these are a few of the most commonly used variables.

## A brief overview of Objects in Python: The "why" of member functions

You have already used **modules** in python. To recap; a module is a
library of related functions which can be **imported** into a script
to access those functions. 

Using a module, e.g. the build-in `os` module to access operating-system 
related functionality, is as simple as adding 
```py
import os
```
to the top of your script. 

Then, a **module function** is accessed using the dot (".") notation: 
```py
os.listdir()
```
would for example return the directory listing for the current working
directory. 

However, you have also seen dot notation used when accessing functions
that we've referred to as **member functions**. A member function 
refers to a function that belongs to an **object**. For example, 
the built-in function `open` returns a **file object**. 

This file object has member functions such as **readlines** which is
used to read the entire contents the file into a list. 

The reason we are talking about **objects** is to stress the difference 
between a module function like `os.listdir`, and a member function 
e.g.  
```py 
the_best_file = open("experiment99.txt")
data = the_best_file.readlines() 
```
Another example of a member function that we've already used is 
`append`, which is a member function of list objects. 
E.g. we saw
```py
list_out = []  # Here we create a list object 
for val in list_in:
    list_out.append(val*val)    # Here we call the member-function "append"
```

As long as we are happy that there are **modules** which are *collections 
of functions*, and independently there are **objects** which ~point to data 
but also have member functions, we're ready to start learning about 
one of the most important libraries available to researchers, Numpy.


## IPython


Lastly, it's worth drawing attention to IPython for those of you who haven't 
tried it yet. 

On WinPython this can be started by starting the IPython Qt Console application. 
If WinPython isn't _registered_ with Windows (as in Hatherly), 
use the file explorer to navigate to the WinPython folder (~ "C:\WinPython") and 
start the "IPython Qt Console" application. 


IPython provides an interactive console which is ideal for testing snippets of code 
and ideas when developing. 


**Please note** however that while IPython is a great _tool_ it should be just that and 
resist the temptation to abandon writing your code in scripts!

If you try and write anything vaguely complex in the IPython console, you would quickly 
find yourself in a pickle when it comes to indentation levels etc. 

In addition variables are all kept in memory making it easy to make mistakes by using a previously
defined variable. 

<div class="important-section">
In short, IPython is a great tool when used correctly - don't abuse it!
</div> 



