Title: Understanding CPython (3.4) Objects (Part 1: The built-in objects)
Date: 2015-07-31 10:00
Category: Python
Author: Jesús Espino
Tags: python, cpython

Introduction
------------

First of all, this article talks about CPython, the implementation of reference
of python language, written in C.

I want to explain how looks like the structure of the python objects, and how
it works.

We have to know that everything in python is an Object, and that an object and
an intance is the same. On the other hand, every object, has a class or type
(is the same in python), and of course, the types are objects too. Let's see it
in the next diagram:

![Python objects relation]({filename}/images/cpython-objects/Ojbect-Type-Relation.svg){:style="width: 60%"}

The built-in classes and your own classes are types too, and instances of the
class "type".

![Python classes relation]({filename}/images/cpython-objects/MyClass-BuiltinClass-Relation.svg){:style="width: 60%"}

Knowing this, you can see that a metaclass is simply a class that acts as the "type" class for your defined class.

![Python metaclasses relation]({filename}/images/cpython-objects/Metaclass-Relation.svg){:style="width: 60%"}

The Object basic structure
--------------------------

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
everything in python, types are objects too). The type of an object define it's
behavior.

![PyObject object structure]({filename}/images/cpython-objects/PyObject.svg){:style="width: 35%"}

There are another object structure for variable size (*PyVarObject*)
which add the *ob_size* field. This field have different meaning depending on the
object type. We will see it better when we see each python object.

![PyVarObject object structure]({filename}/images/cpython-objects/PyVarObject.svg){:style="width: 35%"}

### Interesting links

* [*PyObject* structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/object.h#l105)
* [*PyVarObject* structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/object.h#l111)

The None object
---------------

![None object structure]({filename}/images/cpython-objects/None.svg){:style="width: 35%"}

The *None* object is the simplest object in python, this one only have the
*ob_refcnt* and the *ob_type* and, in the CPython, there's only one instance of
None for all the interpreter. None is a sigleton instance. This is the reason
to use the construct *is* *None* for eficciency, because *is* check if the
object is the same instance (the same address in memory).

### Interesting links

* [CPython None structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Objects/object.c#l1453)

The float object
----------------

![float object structure]({filename}/images/cpython-objects/Float.svg){:style="width: 35%"}

The *float* object is really simple in CPython, it has a *ob_refcnt*, the
*ob_type* and an attribute *ob_fval* which store a C double value. Of course,
the value of the float python object is the value stored in the *ob_fval*
attribute.

### Interesting links

* [CPython float structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/floatobject.h#l15)

The int object
--------------

![int object structure]({filename}/images/cpython-objects/Int.svg){:style="width: 35%"}

The *int* object is more interesting than the *float* object, because have a
little bit complex structure. The *int* object have the *ob_refcnt* and the
*ob_type*, like any other python object, but have an *ob_size* attribute thats
define the number of C int elements will use to represent the value of the
python *int* object. Afther this attribute have an array (called *ob_digit*) of
C integers with the size of *ob_size*. Lets see an example:

![int usage example]({filename}/images/cpython-objects/Int-Usage.svg){:style="width: 60%"}

The value of the *int* python object is calculated using the values stored in
the *ob_digit* array (TODO: WRITE HERE HOW IS CALCULATED THIS VALUE).

### Interesting links

* [CPython int structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/longintrepr.h#l89)
* [PEP 237 -- Unifying Long Integers and Integers](https://www.python.org/dev/peps/pep-0237/)

The bool object
---------------

![bool object structure]({filename}/images/cpython-objects/True-False.svg){:style="width: 70%"}

The *bool* objects are *True* and *False*, two instances for all the
interpreter, each instance have the same structure than an *int* variable, of
course with the *ob_type* pointing to the *bool* *type*. The *True* instance
have the *ob_size* and *ob_digit* equals to *1*, and the *False* instance have
the *ob_size* and *ob_digit* equals to *0*.

### Interesting links

* [CPython False structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Objects/boolobject.c#l176)
* [CPython True structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Objects/boolobject.c#l181)
* [PEP 285 -- Adding a bool type](https://www.python.org/dev/peps/pep-0285/)

The complex object
------------------

![complex object structure]({filename}/images/cpython-objects/Complex.svg){:style="width: 35%"}

The *complex* objects are objects with a real part and a imaginary part. This
objects structure have the *ob_refcnt*, the *ob_type* and the fields *ob_real*
and *ob_imag* which are two C double values.

### Interesting links

* [CPython complex structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/complexobject.h#l10)

The tuple object
----------------

![tuple object structure]({filename}/images/cpython-objects/Tuple.svg){:style="width: 35%"}

The *tuple* object has the *ob_refcnt*, the *ob_type*, an *ob_size* attribute
which store the number of element in the tuple, and an *ob_items*, which is an
array of C pointers to the objects in the tuple. Lets see an example:

[[ TODO: GRAPHIC EXAMPLE OF TUPLE ]]

### Interesting links

* [CPython tuple structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/tupleobject.h#l25)

The list object
---------------

![list object structure]({filename}/images/cpython-objects/List.svg){:style="width: 35%"}

### Interesting links

* [CPython list structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/listobject.h#l23)

The bytes object
----------------

![bytes object structure]({filename}/images/cpython-objects/Bytes.svg){:style="width: 35%"}

![bytes usage example]({filename}/images/cpython-objects/Bytes-Usage.svg){:style="width: 50%"}

### Interesting links

* [CPython bytes structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/bytesobject.h#l31)
* [PEP 3137 -- Immutable Bytes and Mutable Buffer](https://www.python.org/dev/peps/pep-3137/)

The dict object
---------------

![dict object structure]({filename}/images/cpython-objects/Dict.svg){:style="width: 100%"}

### Interesting links

* [CPython dict structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/dictobject.h#l23)
* [CPython dict keys structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Objects/dictobject.c#l87)
* [CPython dict key entry structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Objects/dictobject.c#l77)
* [PEP 0412 -- Key-Sharing Dictionary](https://www.python.org/dev/peps/pep-0412/)

What's next?
------------

In the next part I will explain how works the user defined objects.
