Doing without ``class``
=======================

A talk by Tibs (they/he), for
`CamPUG <https://www.meetup.com/CamPUG/>`__, on `Tuesday 5th October
2021 <https://www.meetup.com/CamPUG/events/280947413/>`__

Sources are at https://github.com/tibs/doing-without-class

Presented using `Jupyter
Notebook <https://jupyter-notebook.readthedocs.io/en/stable/>`__

Licensed under a `Creative Commons Attribution-ShareAlike 4.0
International
License <http://creativecommons.org/licenses/by-sa/4.0/>`__ **except**
for the flowchart image from
https://terribleminds.com/ramble/2013/08/06/are-you-a-real-writer-a-handy-and-hasty-flowchart/
which belongs to Chuck Wendig.

Abstract
--------

When I named this talk, I realised that the name was ambiguous.

I could have changed the name, but instead I figured I’d do both talks.

-  Is it OK to write Python code without classes?
-  Can we create Python classes without using the ``class`` keyword

Part 1: Is it OK to write Python code without classes?
------------------------------------------------------

**tldr;** Yes

Ideas:

-  Write the simplest thing that will get the job done
-  Python is deliberately a multi-paradigm language

`The seven programming
ur-languages <https://madhadron.com/posts/seven_languages.html>`__ by
Fred Ross, 2021

Broadly: ALGOL, Lisp, ML, Self, Forth, APL, Prolog

Python clearly fits well in the “ALGOL” family.

   **Characteristics**. Programs consist of sequences of assignments,
   conditionals, and loops, organized into functions. Many languages add
   module systems, ways of defining new data types, polymorphism, or
   alternate control flow constructs like exceptions or coroutines.

..

   Most common programming languages trace to this ur-language

`Stop Writing
Classes <https://pyvideo.org/pycon-us-2012/stop-writing-classes.html>`__

A talk from PyCon US 2012, by Jack Diederich

   Classes are great but they are also overused. This talk will describe
   examples of class overuse taken from real world code and refactor the
   unnecessary classes, exceptions, and modules out of them.”

There’s nothing wrong with:

1. Write some simple linear code
2. Ooh - functions would make this easier - refactor to use functions
3. Ooh - maybe I need a class - refactor to use it

Some history
^^^^^^^^^^^^

…from a C programmer

Back in the late 1980s, we were writing C code that created a
``context`` datastructure, which we would pass to functions.

This would contain the bundle of data items that most functions needed
most of the time.

We would put it as the first argument to those functions.

Later on, we had the idea to add pointers to common functions to those
contexts.

So we’d typically have ``print`` and ``compare`` function pointers, and
we’d attach the appropriate function *for that particular type of
context*.

Which we’d do in an ``init`` function.

Yes, we too invented some of the basics of OO.

Just like lots of people, I’m sure.

What’s the point?

Well, sometimes it’s OK to pass a context value around, without turning
things “inside out” and creating a class.

And, to be honest, if there are only a few values in that context, it
may not even be worth creating it. Just pass the values.

Some history
^^^^^^^^^^^^

…from a Fortan programmer

Before we used C, we used Fortran. It was somewhere between FORTRAN IV
and Fortran 77.

So a lot of the communication between functions and subroutines was done
with ``COMMON`` blocks - essentially named global storage.

We managed.

Personally, I am a library writer at heart, so am unreasonably
prejudiced against using Python ``global``\ s.

But if you’re writing a simple *program*

-  and all of the functions in that program use a few shared values,
-  and especially if few (if any) of them modify those values

then globals can make sense.

In summary
^^^^^^^^^^

-  It’s OK to keep things simple
-  It’s OK not to use classes if they’re not needed
-  It’s OK to pass around ``context`` values
-  It’s OK (in a program, not in a library!) to use ``global`` values

**But** be prepared to refactor when it gets hard to understand.

Addendum
^^^^^^^^

I think people sometimes ask questions like this because they’re not
sure if they’re “a real programmer” if they don’t tick certain boxes /
do certain things.

I think Chuck Wendig’s flowchart for `“Are you a real
writer” <https://terribleminds.com/ramble/2013/08/06/are-you-a-real-writer-a-handy-and-hasty-flowchart/>`__
is particularly useful - just change “writer” to be “programmer”.



Part 2: Can we create Python classes without using the ``class`` keyword
------------------------------------------------------------------------

(and some other stuff)

Summary:

-  Looking at a simple class
-  Can we define a class without class? (*tldr;* yes)
-  Can we construct an instance by hand? (*tldr;* yes-ish)
-  Can we create a function without using def? (*tldr;* sort-of no)

Looking at a simple class
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython3

    class Example:
        """A simple example class"""
        a = 3
        def f(self, p):
            """A function that takes a single argument"""
            return f'The parameters were {self} and {p}'

Which we can use in the expected manner

.. code:: ipython3

    e = Example()
    print(f'Class {Example}')
    print(f'Instance {e}')
    print(f'Value    {e.a}')
    print(f'Method   {e.f}')
    print(f'Calling the method with 3 {repr(e.f(3))}')


.. parsed-literal::

    Class <class '__main__.Example'>
    Instance <__main__.Example object at 0x10a030a30>
    Value    3
    Method   <bound method Example.f of <__main__.Example object at 0x10a030a30>>
    Calling the method with 3 'The parameters were <__main__.Example object at 0x10a030a30> and 3'


Using values
~~~~~~~~~~~~

We can update the ``a`` on the class and see it also change on the
instance

.. code:: ipython3

    Example.a = 4
    print(Example.a)
    print(e.a)


.. parsed-literal::

    4
    4


But if we update the ``a`` on the instance

.. code:: ipython3

    print(e.a)
    e.a += 1
    print(e.a)


.. parsed-literal::

    4
    5


It does not change ``a`` on the class

.. code:: ipython3

    print(Example.a)


.. parsed-literal::

    4


And now changing it on the class won’t touch the value on the instance

.. code:: ipython3

    Example.a = 99
    print(f'Class    "a" {Example.a}')
    print(f'Instance "a" {e.a}')


.. parsed-literal::

    Class    "a" 99
    Instance "a" 5


If we add a new value to the class, the instance will get it

.. code:: ipython3

    Example.b = 12
    print(f'Class    "b" {Example.b}')
    print(f'Instance "b" {e.b}')


.. parsed-literal::

    Class    "b" 12
    Instance "b" 12


But not the other way round

.. code:: ipython3

    e.c = -1
    print(f'Instance has "c" {hasattr(e, "c")}')
    print(f'Instance "c" {e.c}')
    print(f'Class has "c" {hasattr(Example, "c")}')
    try:
        print(f'Class    "c" {Example.c}')
    except Exception as exc:
        print(f'{exc.__class__}: {exc}')


.. parsed-literal::

    Instance has "c" True
    Instance "c" -1
    Class has "c" False
    <class 'AttributeError'>: type object 'Example' has no attribute 'c'


Aside
^^^^^

Why code that as

.. code:: python

   try:
       <thing>
   except Exception as exc:
       print(f'{exc.__class__}: {exc}')

rather than let the exception “just happen”?

Purely pragmatism - I expect the exception, and if I don’t catch it we
get to see the traceback (which is generally a Good Thing) but if I try
to run all of the notebook using “Cells / Run All”, it will stop at the
first traceback, which is inconvenient when testing the talk.

Using methods
~~~~~~~~~~~~~

We can call the method on the instance:

.. code:: ipython3

    e.f(4)




.. parsed-literal::

    'The parameters were <__main__.Example object at 0x10a030a30> and 4'



Or via the class, although then we need to pass in the instance
(``self``) explicitly

.. code:: ipython3

    Example.f(e, 4)




.. parsed-literal::

    'The parameters were <__main__.Example object at 0x10a030a30> and 4'



and there’s nothing special about ``self``

.. code:: ipython3

    Example.f('not an instance', 4)




.. parsed-literal::

    'The parameters were not an instance and 4'



So we shouldn’t be surprised that on the class it’s a function,

but on the instance it’s a method,

which gets passed the instance as its first argument

.. code:: ipython3

    print(f'Example.f is a {type(Example.f)}')
    print(f'e.f is a {type(e.f)}')


.. parsed-literal::

    Example.f is a <class 'function'>
    e.f is a <class 'method'>


Aside
^^^^^

In Python, all methods are functions, and there’s nothing special about
the name ``self``.

.. code:: ipython3

    def double(self, x): return x + x
    
    Example.double = double
    print(f'Class "Example" has "double" {hasattr(Example, "double")}')
    print(f'Example.double is a {type(Example.double)}')
    print(f'Example.double("thing", 3) is {Example.double("thing", 3)}')
    
    print(f'Instance "e" has "double" {hasattr(e, "double")}')
    print(f'e.double is a {type(e.double)}')
    print(f'e.double(3) is {e.double(3)}')


.. parsed-literal::

    Class "Example" has "double" True
    Example.double is a <class 'function'>
    Example.double("thing", 3) is 6
    Instance "e" has "double" True
    e.double is a <class 'method'>
    e.double(3) is 6


Contrariwise, in Ruby, all functions are methods:

.. code:: ruby

   irb(main):005:1* def fn(a)
   irb(main):006:1*   puts "self is #{self} #{self.inspect}"
   irb(main):007:0> end
   => :fn
   irb(main):008:0> fn(1)
   self is main main
   => nil

It seems natural to me that in Python we have to specify the ``self``
argument explicitly, as it’s not special *to the function*.

However, in Ruby, as all functions are methods, there’s always a parent
class, and always a ``self``, so there’s no need to put it in the
argument list.

End of aside
^^^^^^^^^^^^

Can we define a class without ``class``?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Can we create a class without using the ``class`` keyword?

We’ve already used the ``type`` callable:

.. code:: ipython3

    type(Example)




.. parsed-literal::

    type



(Although I would have guessed a class would be of type “class”)

and of course

.. code:: ipython3

    print(type(1))
    print(type('2'))


.. parsed-literal::

    <class 'int'>
    <class 'str'>


While ``type`` looks like a function, interestingly it isn’t

If we ask for ``help(type)``, we see:

::

   Help on class type in module builtins:

   class type(object)
    |  type(object_or_name, bases, dict)
    |  type(object) -> the object's type
    |  type(name, bases, dict) -> a new type
    ...

We’ve been using the first of those

``type(object) -> the object's type``.

and now we want the second

``type(name, bases, dict) -> a new type``

.. code:: ipython3

    EmptyClass = type('EmptyClass', (), {})

.. code:: ipython3

    print(type(EmptyClass))
    print(repr(EmptyClass))


.. parsed-literal::

    <class 'type'>
    <class '__main__.EmptyClass'>


.. code:: ipython3

    ec = EmptyClass()
    print(type(ec))
    print(repr(ec))


.. parsed-literal::

    <class '__main__.EmptyClass'>
    <__main__.EmptyClass object at 0x10a03f220>


The middle argument specifies base classes

.. code:: ipython3

    AnInteger = type('AnInteger', (int,), {})
    x = AnInteger()
    print(x)


.. parsed-literal::

    0


The last argument can be used to set values on the new class

.. code:: ipython3

    ClassWithValues = type('ClassWithValues', (), {'a': 99, 'f': lambda self: 1})
    cv = ClassWithValues()
    print(cv.a)
    print(cv.f())


.. parsed-literal::

    99
    1


And we can use both if we want

.. code:: ipython3

    def double(x): return x * 2
    DoublingInteger = type('DoublingInteger', (int,), {'double': double})
    db = DoublingInteger(9)
    print(db)
    print(db.double())


.. parsed-literal::

    9
    18


Let’s build a simple class

.. code:: ipython3

    def function_f(self, p):
        """A function we shall use as a method that takes a single argument"""
        return f'The parameters were {self} and {p}'

.. code:: ipython3

    ByHand = type('ByHand', (), {'f': function_f, 'a': 3})

.. code:: ipython3

    bh = ByHand()
    print(bh)
    print(bh.a)
    print(bh.f(2))


.. parsed-literal::

    <__main__.ByHand object at 0x10a03fd00>
    3
    The parameters were <__main__.ByHand object at 0x10a03fd00> and 2


The obvious next thing to do is to make a function to make classes

.. code:: ipython3

    from collections import ChainMap
    def make_a_class(name, value_dict, method_dict):
        cls = type(name, (), dict(ChainMap(value_dict, method_dict)))
        return cls

.. code:: ipython3

    C = make_a_class('ByHand', {'a': 3}, {'f': function_f})
    print(f'Class {C!r}')
    print(f'Class value a {C.a!r}')
    print(f'Class function {C.f(None, "fred")}')


.. parsed-literal::

    Class <class '__main__.ByHand'>
    Class value a 3
    Class function The parameters were None and fred


We can create an instance, just as we might expect

.. code:: ipython3

    o = C()
    print(f'Instance {o!r}')
    print(f'Instance vaue a {o.a!r}')
    print(f'Instance function {o.f("fred")}')


.. parsed-literal::

    Instance <__main__.ByHand object at 0x10a037b50>
    Instance vaue a 3
    Instance function The parameters were <__main__.ByHand object at 0x10a037b50> and fred


Can we construct an instance by hand?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Can we create an empty object and add things to it?

Our first guess might be to create an instance of the base class,
``Object``

.. code:: ipython3

    o = object()

but unfortunately, its not possible to add new values to instances of
``Object``

.. code:: ipython3

    try:
        o.a = 1
    except Exception as exc:
        print(f'{exc.__class__}: {exc}')


.. parsed-literal::

    <class 'AttributeError'>: 'object' object has no attribute 'a'


So we still have to use ``type`` to get an empty mutable object

.. code:: ipython3

    EmptyClass = type('EmptyClass', (), {})
    eo = EmptyClass()
    print(type(eo))


.. parsed-literal::

    <class '__main__.EmptyClass'>


And we know we can do

.. code:: ipython3

    eo.a = 1
    print(eo.a)


.. parsed-literal::

    1


.. code:: ipython3

    eo.a = eo.a + 1
    print(eo.a)


.. parsed-literal::

    2


Can we add a function *to the object* and have it be a method?

.. code:: ipython3

    def maybe_a_method(self, x):
        print(f'Maybe a method on {self} and {x}')
        
    eo.f = maybe_a_method
    try:
        print(eo.f(1))
    except Exception as exc:
        print(f'{exc.__class__}: {exc}')


.. parsed-literal::

    <class 'TypeError'>: maybe_a_method() missing 1 required positional argument: 'x'


Unfortunately, adding a function as a value on an instance doesn’t make
a method

.. code:: ipython3

    print(type(eo.f))


.. parsed-literal::

    <class 'function'>


Normally, when we ask an instance for a method (``eo.f``), it gets
looked up in the instance, isn’t found there, and is looked up in the
class, which says “I know what you’re doing, that’s a function you’re
looking up on me, so you must want a method back”

.. code:: ipython3

    class NoMethods: pass
    def just_a_function(self): return 'Aha!'
    NoMethods.f = just_a_function
    nm = NoMethods()
    
    print(type(just_a_function))
    print(type(NoMethods.f))
    print(type(nm.f))
    print(nm.f())


.. parsed-literal::

    <class 'function'>
    <class 'function'>
    <class 'method'>
    Aha!


But our empty object is an instance of an empty class, so that won’t
work.

Luckily there is a way:

.. code:: ipython3

    eo.f = just_a_function.__get__(eo, EmptyClass)
    print(type(eo.f))
    print(eo.f())


.. parsed-literal::

    <class 'method'>
    Aha!


I don’t propose to explain that (but am grateful to
https://stackoverflow.com/a/46757134 for the example).

If you want to learn more, then this is using the power of
*descriptors*, which are at the heart of Python’s attribute access

See the HOWTO at https://docs.python.org/3/howto/descriptor.html

There is also a more “understandable” way to do this.

We can create a ``method`` from our function

.. code:: ipython3

    import types
    eo.f = types.MethodType(just_a_function, eo)
    print(type(eo.f))
    print(eo.f())


.. parsed-literal::

    <class 'method'>
    Aha!


And we can create a function to wrap this nicely

.. code:: ipython3

    def pretend_instance(class_name, variable_dict, function_list):
        eo_class = type(class_name, (), variable_dict)
        eo = eo_class()
        for f in function_list:
            setattr(eo, f.__name__, types.MethodType(f, eo))
        return eo

.. code:: ipython3

    x = pretend_instance('ClassName', {'var': 3}, [just_a_function])
    print(type(x))
    print(x.var)
    print(x.just_a_function())


.. parsed-literal::

    <class '__main__.ClassName'>
    3
    Aha!


Can we create a function without using ``def``?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Well, there is lambda

.. code:: ipython3

    lamb = lambda x: x + 1
    
    lamb(2)




.. parsed-literal::

    3



although

1. That’s another keyword
2. It’s very limited in what it can do

..

   An anonymous inline function consisting of a single expression which
   is evaluated when the function is called

There is also ``types.FunctionType``, which is similar in idea to our
use of ``type`` to create classes.

(unfortunately, its signature is implementation specific and may even
change between Python versions)

.. code:: python

   >>> help(types.FunctionType)
   Help on class function in module builtins:

   class function(object)
    |  function(code, globals, name=None, argdefs=None,
    |           closure=None)
    |
    |  Create a function object.
    |
    |  code
    |    a code object
    |  globals
    |    the globals dictionary
    ...

We can get a code object from an existing function

.. code:: ipython3

    print(lamb.__code__)


.. parsed-literal::

    <code object <lambda> at 0x10a05d7c0, file "/var/folders/_z/6hyb5fwx3rx565ssb7ggxl680000gn/T/ipykernel_26467/2561191998.py", line 1>


.. code:: ipython3

    import types
    lambish = types.FunctionType(lamb.__code__, globals())
    print(lambish(1))


.. parsed-literal::

    2


or we can create one with ``compile``

.. code:: ipython3

    code = compile('print(4)', 'no-file', 'exec')
    compiled_fn = types.FunctionType(code, globals())
    print(compiled_fn())


.. parsed-literal::

    4
    None


But how did ``lambish`` know about its argument?

.. code:: ipython3

    print(dir(lamb.__code__))


.. parsed-literal::

    ['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'co_argcount', 'co_cellvars', 'co_code', 'co_consts', 'co_filename', 'co_firstlineno', 'co_flags', 'co_freevars', 'co_kwonlyargcount', 'co_lnotab', 'co_name', 'co_names', 'co_nlocals', 'co_posonlyargcount', 'co_stacksize', 'co_varnames', 'replace']


The documentation for the ``inspect`` module tells us that
``co_argcount`` is the number of arguments, and ``co_varnames`` is a
tuple of the names of the arguments and then the names of the local
variables

.. code:: ipython3

    print(lamb.__code__.co_argcount)
    print(lamb.__code__.co_varnames)


.. parsed-literal::

    1
    ('x',)


But that’s not mutable

.. code:: ipython3

    try:
        lamb.__code__.co_varnames = ('x', 'y')
    except Exception as exc:
        print(f'{exc.__class__}: {exc}')


.. parsed-literal::

    <class 'AttributeError'>: readonly attribute


And at this point, I haven’t been able to find how to define the
arguments for a ``code`` value.

Some other possibly relevant things
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Looking at bytecode
^^^^^^^^^^^^^^^^^^^

We can look at the bytecode for a callable
(https://docs.python.org/3/library/dis.html)

.. code:: ipython3

    import dis
    dis.dis(lambish)


.. parsed-literal::

      1           0 LOAD_FAST                0 (x)
                  2 LOAD_CONST               1 (1)
                  4 BINARY_ADD
                  6 RETURN_VALUE


That should be fairly recognisable as our lambda function.

Unsurprisingly, what that did was look up the ``__code__`` value on
``lambish`` for us

.. code:: ipython3

    dis.dis(lamb.__code__)


.. parsed-literal::

      1           0 LOAD_FAST                0 (x)
                  2 LOAD_CONST               1 (1)
                  4 BINARY_ADD
                  6 RETURN_VALUE


Clearly one could construct a function “by hand” as bytecode.

But it would be laborious, specific to CPython, and the bytecode is not
guaranteed to remain the same between Python versions.

As an example of bytecode manipulation,
https://github.com/snoack/python-goto provides a decorator that rewrites
the bytecode for a function to allow the use of ``goto`` within the
function.

The ``inspect`` module
^^^^^^^^^^^^^^^^^^^^^^

The ``inspect`` module (https://docs.python.org/3/library/inspect.html)
has many useful ways to introspect Python code. Here are a few

Loooking up the signature for a callable

.. code:: ipython3

    import inspect
    print(inspect.signature(lambish))


.. parsed-literal::

    (x)


Getting the source code for an object

.. code:: ipython3

    print(inspect.getsource(Example.f))


.. parsed-literal::

        def f(self, p):
            """A function that takes a single argument"""
            return f'The parameters were {self} and {p}'
    


Asking about the general kind of an object

.. code:: ipython3

    print(inspect.isfunction(just_a_function))
    print(inspect.ismethod(just_a_function))
    print(inspect.ismethod(e.f))
    print(inspect.isclass(make_a_class('ClassThing', {'a': 3}, {})))


.. parsed-literal::

    True
    False
    True
    True


The ``ast`` module
~~~~~~~~~~~~~~~~~~

Python code is translated into an Abstract Syntax Tree, from which the
byte code is generated.

The ``ast`` module allows manipulating such ASTs.

So it seems plausible that one should be able to construct functions,
from scratch, using this.

https://gist.github.com/dhagrow/d3414e3c6ae25dfa606238355aea2ca5 gives
code to create a function using the AST, although it needs a previous
function to “copy”. It supports Python2 and Python3.

Hy (https://github.com/hylang/hy) is a Lisp dialect embedded in Python.
It transforms its Lisp AST into a Python AST.

Fin
~~~

A talk by Tibs (they/he), for
`CamPUG <https://www.meetup.com/CamPUG/>`__, on `Tuesday 5th October
2021 <https://www.meetup.com/CamPUG/events/280947413/>`__

Sources are at https://github.com/tibs/doing-without-class

Presented using `Jupyter
Notebook <https://jupyter-notebook.readthedocs.io/en/stable/>`__

Licensed under a `Creative Commons Attribution-ShareAlike 4.0
International
License <http://creativecommons.org/licenses/by-sa/4.0/>`__ **except**
for the flowchart image from
https://terribleminds.com/ramble/2013/08/06/are-you-a-real-writer-a-handy-and-hasty-flowchart/
which belongs to Chuck Wendig.

Some useful links
~~~~~~~~~~~~~~~~~

(this is too long to display sensibly as a slide)

If you’re interested in how Python works, then the book `Fluent
Python <https://smile.amazon.co.uk/Fluent-Python-Luciano-Ramalho/dp/1491946008>`__
by Luciano Ramalho is excellent.

If you want to dig into how CPython is implemented, then `CPython
Internals: Your Guide to the Python 3
Interpreter <https://smile.amazon.co.uk/CPython-Internals-Guide-Python-Interpreter/dp/1775093344>`__
by Anthony Shaw looks excellent (I’ve still to read it properly).

I found the following to be useful while researching this:

-  https://stackoverflow.com/questions/19476816/creating-an-empty-object-in-python
   has interesting takes on creating an empty object
-  https://stackoverflow.com/questions/13184281/python-dynamic-function-creation-with-custom-names
   taks about ways to use inline ``def`` to construct functions, and
   there’s a link to https://smarie.github.io/python-makefun/, which
   seems to be a comprehensive solution for constructing functions at
   runtime (I’ve not used it)
-  https://stackoverflow.com/questions/394770/override-a-method-at-instance-level
   has a discussion on why assigning a function to an instance doesn’t
   set it on the class. The `answer by Mad
   Physicist <https://stackoverflow.com/a/46757134>`__ was particularly
   useful.

If you’re interested in ``descriptors``, Raymond Hettinger’s `Descriptor
HowTo Guide <https://docs.python.org/3/howto/descriptor.html>`__ is
excellent (I tend to recommend everything by him).

In the Python documentation, we’ve referenced, at least in passing:

-  https://docs.python.org/3/library/types.html describes the ``types``
   module, “Dynamic type creation and names for built-in types”
-  https://docs.python.org/3/library/functions.html#object describes the
   ``object`` callable
-  https://docs.python.org/3/library/functions.html#type describes the
   ``type`` callable
-  https://docs.python.org/3/library/functions.html#compile describes
   the ``compile`` function, to produce byte code from source code
-  https://docs.python.org/3/library/inspect.html describes the
   ``inspect`` module, which is worth a read just to see what you can
   find out about objects
-  https://docs.python.org/3/library/dis.html describes the ``dis``
   module, and how to disassemble callables - honestly, something you
   shouldn’t *need* to do, but it can be interesting
-  https://docs.python.org/3/library/ast.html describes the ``ast``
   module

Other links from the slides:

-  `The seven programming
   ur-languages <https://madhadron.com/posts/seven_languages.html>`__ by
   Fred Ross, 2021
-  `Stop Writing
   Classes <https://pyvideo.org/pycon-us-2012/stop-writing-classes.html>`__
   talk from PyCon US 2012, by Jack Diederich
-  Chuck Wendig’s flowchart for `“Are you a real
   writer” <https://terribleminds.com/ramble/2013/08/06/are-you-a-real-writer-a-handy-and-hasty-flowchart/>`__
-  https://github.com/snoack/python-goto
-  https://gist.github.com/dhagrow/d3414e3c6ae25dfa606238355aea2ca5
   gives code to create a function using the AST
-  https://github.com/hylang/hy - Lisp for Python

