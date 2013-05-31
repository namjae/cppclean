========
cppclean
========
.. image:: https://travis-ci.org/myint/cppclean.png?branch=master
   :target: https://travis-ci.org/myint/cppclean
   :alt: Build status


Goal
====
cppclean attempts to find problems in C++ source that slow development
in large code bases, for example various forms of unused code.
Unused code can be unused functions, methods, data members, types, etc
to unnecessary #include directives. .nnecessary #includes can cause
considerable extra compiles increasing the edit-compile-run cycle.

The project home page is: http://code.google.com/p/cppclean/


Features
========
* Find and print C++ language constructs: classes, methods, functions, etc.
* Find classes with virtual methods, no virtual destructor, and no bases
* Find global/static data that are potential problems when using threads
* Unnecessary forward class declarations
* Unnecessary function declarations
* Undeclared function definitions
* (planned) Find unnecessary header files #included
    - No direct reference to anything in the header
    - Header is unnecessary if classes were forward declared instead
* (planned) Source files that reference headers not directly #included,
   ie, files that rely on a transitive #include from another header
* (planned) Unused members (private, protected, & public) methods and data
* (planned) Store AST in a SQL database so relationships can be queried

AST is Abstract Syntax Tree, a representation of parsed source code.
http://en.wikipedia.org/wiki/Abstract_syntax_tree


System Requirements
===================
 * Python 2.4 or later (2.3 probably works too)
 * Works on Windows (untested), Mac OS X, and Unix


Installation
============
::

    $ pip install --upgrade git+https://github.com/myint/cppclean.git


How to Run
==========
::

    $ cppclean <path>


How to Configure
================
You can add a siteheaders.py file in /cppclean/cpp to configure where
to look for other headers (typically -I options passed to a compiler).
Currently two values are supported: .TRANSITIVE and GetIncludeDirs.
_TRANSITIVE should be set to a boolean value (True or False) indicating
whether to transitively process all header files. .he default is False.

GetIncludeDirs is a function that takes a single argument and returns
a sequence of directories to include. .his can be a generator or
return a static list::

    def GetIncludeDirs(filename):
        return ['/some/path/with/other/headers']

    # Here is a more complicated example.
    def GetIncludeDirs(filename):
        yield '/path1'
        yield os.path.join('/path2', os.path.dirname(filename))
        yield '/path3'


Current Status
==============
The parser works pretty well for header files, parsing about 99% of Google's
header files. Anything which inspects structure of C++ source files should
work reasonably well. Function bodies are not transformed to an AST,
but left as tokens. .uch work is still needed on finding unused header files
and storing an AST in a database.


Non-goals
=========
* Parsing all valid C++ source
* Handling invalid C++ source gracefully
* Compiling to machine code (or anything beyond an AST)


Contact
=======
If you used cppclean, I would love to hear about your experiences
cppclean@googlegroups.com. .ven if you don't use cppclean, I'd like to
hear from you. :-) (You can contact me directly at: nnorwitz@gmail.com)