---
title: Python Monkey Patching
date: 2022-02-18 21:22:42 +05:30
tags: [HowTo, Python]
description: A short, simple  and funny guide to Python Monkey Patching.
image: "/monkey-patching/monkey.jpg"
comments: true
---
<figure>
<img src="monkey.jpg" alt="You are not allowed!"> 
<figcaption style="color: grey !important;"> 
	Photo by <a href="https://unsplash.com/@andremouton" style="color: grey !important;" target="_blank">Andre Mouton
</a> 
</figcaption>
</figure>

Python is a characteristic dynamic scripting language. Not only it has dynamic type are but it's object model is also dynamic. Python's classes are mutable, and methods are just attributes of the class; This allows us to modify its behavior at run time. This is funnily called *Monkey Patching*, with possible reference to *guerrilla patch* which referred to changing code sneakily. 


Monkey Patching is simply the dynamic replacement of attributes at runtime. In Python, the term monkey patch refers to dynamic (or run-time) modifications of a function, class or module. Let's understand with an example, we use a python package wich has a class like this:
```
	# monkey.py
	class Me:
	     def who_am_i(self):
	         print ("I am a Monkey")
```
Now since I am a human, I don't like this so I Monkey Patch \[pun intended\].
```
	import monkey
	def i_am_human(self):
	    print ("I am human")
	   
	# replacing address of "who_am_i" with "i_am_human"
	monkey.Me.who_am_i = i_am_human
	obj = monkey.Me()
	  
	# calling function "who_am_i" whose address got replaced
	# with function "i_am_human()"
	obj.who_am_i()
```
This will print "I am a human", which is corret; But it need a bit of *Monkey Patching*.


## Mutable and Immutable Data Types
To understand monkey patching we need to understand the differences between mutable and immutable data types in Python. We can think about variables in Python as labels instead of boxes. In Python, a variable is a label that we assign to an object; it is the way we, as humans, have to identify it. However, what is important is the data underlying the label, its value, and its type. Custom objects are mutable, and therefore their attributes can be replaced without creating a new copy of the object. so `float, decimal, complex, bool, string, tuple, range, frozenset, bytes` etc. are Mutable while `list`, `dictionary`, `set`, `bytearray`, *user-defined classes* are Immutable. Here we are interested in immutable object, becausethey can be changed in-place.


## Patching an Instance
Above instance patched a class method and all instances of the class will have the patched method. We can also patch a specific instance. 
```
import types
import monkey

monkey1 = monkey.Me()
monkey2 = monkey.Me()

def i_am_human(self):
	print ("I am human")

monkey2.who_am_i = types.MethodType(i_am_human, monkey2)

print(monkey1.who_am_i())
# I am a Monkey
print(monkey2.get_value())
# I am a human
```

So with the use of types, `monkey2` has has been patched to become a human. 


## Patching a Module

```
	# whoami.py

	def who_am_i(monkey_type):
	    print (f"I am a {monkey_type}")
```
```
	# human.py
	import whoami

	def who_am_i():
	    whoami.who_am_i("Human")
```
```
	# monkey.py
	import whoami
	import human

	def i_am_monkey(self):
	    who_am_i("Monkey")

	 whoami.who_am_i = i_am_monkey
	 human.who_am_i()
	 # I am a Monkey

```
The monkey has hacked the human module by monkey patching. 

## Word of Caution
 Monkey patching is very powerful. It’s useful if we are dealing with legacy code or code from other people in which we do not want to modify it extensively but still want to make it run with different versions of libraries or environments. 

> As a general rule, the best is not to monkey patch. 

 The problem with monkey-patching is that the behavior of a program becomes much harder to understand. In above example soon enough it will be confusing "Who is Monkey" and "Who is Human". Whereever possible extend the public interface of third party library.

Sometimes there Monkey Patching can be a great benefit. You could replace all instance of a method with another with just one line of code change. Monkey patching is also useful in testing, when you want to use a mock method to test. 


## Using Gorrilla Package [](https://pypi.org/project/gorilla/){:target="_blank"}
I like gorrilla, while monkey patching, it makes the process both intuitive and convenient even when faced with large numbers of patches to create.  Here is an example:
```
>>> import gorilla
>>> import destination
>>> @gorilla.patches(destination.Class)
... class MyClass(object):
...     def method(self):
...         print("Hello")
...     @classmethod
...     def class_method(cls):
...         print("world!")
```
The code above creates two patches, one for each member of the class MyClass, but does not apply them yet. In other words, they define the information required to carry on the operation but are not yet inserted into the specified destination class destination.Class.

Such patches created with the decorators can then be automatically retrieved by recursively scanning a package or a module, then applied:

```
>>> import gorilla
>>> import mypackage
>>> patches = gorilla.find_patches([mypackage])
>>> for patch in patches:
...     gorilla.apply(patch)
```
You can find more in [Gorilla Documentation](https://gorilla.readthedocs.io/en/latest/){:target="_blank"}
```