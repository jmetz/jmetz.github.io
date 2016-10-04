---
collection: intropython2016
title: Getting Started 
layout: tutorial 
---
# Getting started

Before we dive into Python, let's get familiar with the environment 
we are going to use to program and run Python. 
The two main components you will need to use Python are 

* A terminal/console
* A text editor


## An editor 

As Python code is human readable text, we need a text editor of some sort 
to read end edit Python code. 

> **Jargon**
>
> If the text editor has features like syntax highlighting (colour coding 
> words in the code based on what they are), 
> code completion, and other goodies, it's called a **code editor**. 
> If it is embedded in an interface that also has a terminal and sometimes a variable browser, 
> the whole program is referred to as an Integrated Development Environment (IDE). 

For this workshop we will be using **Notepad++** as this is similar to Notepad
but adds things like syntax highlighting. 

In the Start Menu, find **Notepad++**, either by looking through the programs
or by using the search field. 

## The terminal

In order to run Python scripts we will use a pre-configured 
command prompt provided by WinPython. 

In the Start Menu, find **WinPython Command Prompt**, either by 
looking through the programs or by using the search field. 
 
To run a script from the terminal use

```shell
python scriptname.py
```

# Getting help

As well as asking the demonstrators, you are encouraged to 
get used to using online resources. Simply searching for e.g. 

python FUNCTIONNAME

<python-search></python-search>

(and replacing FUNCTIONNAME with the name of the function you want help on!)
using your favourite search engine will almost always return relevant 
help. 

While the demonstrators are there to help you get started and provide detailed 
help when you need it, 
it will be very beneficial to you in the long run to become familiar with 
what online sources there are and how to optimize your searches to most 
quickly find the answers you need. 

Resources I often use are: 

* Official Python documentation (very extensive!) : <a href="https://docs.python.org/3/" target="_blank">https://docs.python.org/3/</a>
* Stack overflow (Programmer Q&A; Python tag) : <a href="http://stackoverflow.com/questions/tagged/python" target="_blank">http://stackoverflow.com/questions/tagged/python</a>
* Various blogs e.g. 
    * Planet Python     : <a href="http://planetpython.org/" target="_blank">http://planetpython.org/</a>
    * Doug Hellmann     : <a href="https://doughellmann.com/blog/" target="_blank">https://doughellmann.com/blog/</a>
    * Effbot            : <a href="http://effbot.org/" target="_blank">http://effbot.org/</a>
    * Mouse vs Python   : <a href="http://www.blog.pythonlibrary.org/" target="_blank">http://www.blog.pythonlibrary.org/</a>


> **Advanced Users**
>
> Another simple way of getting help is to use the interactive help 
> system in the IPython console. 
> The IPython console is an interactive Python session, i.e. it looks 
> like a terminal but instead of accepting terminal commands, it 
> accepts Python code directly. 
> The IPython console has several useful features to get help including
> 
> * `help(FUNCTIONNAME)` prints help on the function called `FUNCTIONNAME`
> * `FUNCTIONNAME?` prints help on the function called `FUNCTIONNAME`
> * `MODULENAME.` and then pressing tab (twice) shows a list of all functions available in the module called MODULENAME (if it's imported). 

