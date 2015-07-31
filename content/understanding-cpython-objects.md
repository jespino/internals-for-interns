Title: Understanding CPython (3.4) Objects (Part 1: The built-in objects)
Date: 2015-07-31 10:00
Category: Python
Author: Jesús Espino

First of all, this article talks about CPython, the implementation of reference
of python language, written in C.

I want to explain how looks like the structure of the python objects, and how
it works.

Any object in python is a data struct stored in any point of the process
memory. In CPython every object has an *id* this *id* is the address in memory
of the object, and you can get it with the *id* built-in function.

You have to understand the basic structure of every object in
python, each object in python have at least 2 fields, the *ob_refcnt* and the
*ob_type*.

The *ob_refcnt* is the reference counter of the object, this is the primary
mechanism of python garbage collection. When the *ob_refcnt* of an object
reaches 0, the object is freeded.

The *ob_type* is a C pointer to the address where is stored the type object (as
everything in python, types are objects too).

Now knowing this, we can view some of the main built-in objects, how are
structured and how store the data.

The None object
---------------

The *None* object is the simplest object in python, this one only have the
*ob_refcnt* and the *ob_type* and, in the CPython, there's only one instance of
None for all the interpreter. None is a sigleton instance. This is the reason
to use the construct *is* *None* for eficciency, because *is* check if the
object is the same instance (the same address in memory).

The bool object
---------------

The int object
--------------

The float object
----------------

The complex object
------------------

The list object
---------------

The tuple object
----------------

The bytes object
----------------

The dict object
---------------
