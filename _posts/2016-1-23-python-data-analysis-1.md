# Python 2016: Advanced Python & Data Analysis

[![XKCDSW comic](/images/python.png)](http://xkcdsw.com/77)

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
