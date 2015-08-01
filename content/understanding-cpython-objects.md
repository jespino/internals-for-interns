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
everything in python, types are objects too). The type of an object define it's
behavior.

This two fields are defined in the [*PyObject*
structure](https://hg.python.org/cpython/file/b4cbecbc0781/Include/object.h#l105),
and there are another object structure for [variable size objects
(*PyVarObject*)](https://hg.python.org/cpython/file/b4cbecbc0781/Include/object.h#l111),
which add the *ob_size* field, which have different meaning depending on the
object type. We will see it better when we see each python object.

Now knowing this, we can view some of the main built-in objects, how are
structured and how store the data.

The None object
---------------

![None object structure]({filename}/images/cpython-objects/None.svg){:style="width: 35%"}

[CPython None structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Objects/object.c#l1453)

The *None* object is the simplest object in python, this one only have the
*ob_refcnt* and the *ob_type* and, in the CPython, there's only one instance of
None for all the interpreter. None is a sigleton instance. This is the reason
to use the construct *is* *None* for eficciency, because *is* check if the
object is the same instance (the same address in memory).

The float object
----------------

![float object structure]({filename}/images/cpython-objects/Float.svg){:style="width: 35%"}

[CPython float structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/floatobject.h#l15)

The *float* object is really simple in CPython, it has a *ob_refcnt*, the
*ob_type* and an attribute *ob_fval* which store a C double value. Of course,
the value of the float python object is the value stored in the *ob_fval*
attribute.

The int object
--------------

![int object structure]({filename}/images/cpython-objects/Int.svg){:style="width: 35%"}

[CPython int structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/longintrepr.h#l89)

The *int* object is more interesting than the *float* object, because have a
little bit complex structure. The *int* object have the *ob_refcnt* and the
*ob_type*, like any other python object, but have an *ob_size* attribute thats
define the number of C int elements will use to represent the value of the
python *int* object. Afther this attribute have an array (called *ob_digit*) of
C integers with the size of *ob_size*. Lets see an example:

![int usage example]({filename}/images/cpython-objects/Int-Usage.svg){:style="width: 60%"}

The value of the *int* python object is calculated using the values stored in
the *ob_digit* array (TODO: WRITE HERE HOW IS CALCULATED THIS VALUE).

The bool object
---------------

![bool object structure]({filename}/images/cpython-objects/True-False.svg){:style="width: 70%"}

[CPython False structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Objects/boolobject.c#l176)
[CPython True structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Objects/boolobject.c#l181)

The *bool* objects are *True* and *False*, two instances for all the
interpreter, each instance have the same structure than an *int* variable, of
course with the *ob_type* pointing to the *bool* *type*. The *True* instance
have the *ob_size* and *ob_digit* equals to *1*, and the *False* instance have
the *ob_size* and *ob_digit* equals to *0*.

The complex object
------------------

![complex object structure]({filename}/images/cpython-objects/Complex.svg){:style="width: 35%"}

[CPython complex structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/complexobject.h#l10)

The *complex* objects are objects with a real part and a imaginary part. This
objects structure have the *ob_refcnt*, the *ob_type* and the fields *ob_real*
and *ob_imag* which are two C double values.

The tuple object
----------------

![tuple object structure]({filename}/images/cpython-objects/Tuple.svg){:style="width: 35%"}

[CPython tuple structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/tupleobject.h#l25)

The *tuple* object has the *ob_refcnt*, the *ob_type*, an *ob_size* attribute
which store the number of element in the tuple, and an *ob_items*, which is an
array of C pointers to the objects in the tuple. Lets see an example:

[[ TODO: GRAPHIC EXAMPLE OF TUPLE ]]

The list object
---------------

![list object structure]({filename}/images/cpython-objects/List.svg){:style="width: 35%"}

[CPython list structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/listobject.h#l23)


The bytes object
----------------

![bytes object structure]({filename}/images/cpython-objects/Bytes.svg){:style="width: 35%"}

[CPython bytes structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/bytesobject.h#l31)

The dict object
---------------

![dict object structure]({filename}/images/cpython-objects/Dict.svg){:style="width: 100%"}

[CPython dict structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Include/dictobject.h#l23)
[CPython dict keys structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Objects/dictobject.c#l87)
[CPython dict key entry structure code](https://hg.python.org/cpython/file/b4cbecbc0781/Objects/dictobject.c#l77)


What's next?
------------

In the next part I will explain how works the user defined objects.
