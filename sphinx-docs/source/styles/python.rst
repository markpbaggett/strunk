======
Python
======

----------
Background
----------

Python is one of the primary languages used by Digital Initiatives. This style guide is a list of dos and don'ts for new
Python programs, justifications for these decisions, and examples where necessary.

--------------
Language Rules
--------------

String Concatenation
====================

The rule
--------

There are four ways in Python to concatenate strings:

* "Old-style" string formatting aka %-formatting
* "New-style" string formatting aka .format() formatting
* Literal String Interpolation aka f'strings'
* Template string formatting aka from string import Template

In order to avoid Python files that use all of these forms of concatenation (or even worse "Mark " + "Baggett" style
formatting), we have a set of general rules we follow.

1. If you're taking user supplied strings from an untrusted user and security is a concern, use Template string
   formatting.
2. If the above is not a concern and you only need to support 3.6 and above, use Literal String Interpolation
   aka f'strings'.
3. If you have to support 2.7 through 3.5, use "New-style" formatting aka .format() formatting.
4. Never use "Old-style."

When to use Template strings
----------------------------

Template strings are preferred **ONLY** when you need to take input into your program from untrusted users.

Template strings are not very powerful and have to be imported from standard library. They are not cool, but they are
secure.  Use them, but only when you absolutely need to do so.

>>> from string import Template
>>> t = Template('Hey, $name!')
>>> t.substitute(name=name)
'Hey, Mark!'

That being said, they are the most secure method of concatenation because they are limited in what they can do.  Other
forms might allow users to access arbitrary variables assigned in your program like API keys.

For example:

>>> SECRET = 'this-is-a-secret'
>>> class Error:
...
def __init__(self):
...
pass
>>> err = Error()
>>> user_input = '{error.__init__.__globals__[SECRET]}'
# Uh-oh...
>>> user_input.format(error=err)
'this-is-a-secret'

Yikes. This can be avoided with this:

>>> user_input = '${error.__init__.__globals__[SECRET]}'
>>> Template(user_input).substitute(error=err)
ValueError:
"Invalid placeholder in string: line 1, col 1"

Again, only use if absolutely necessary.


Literal String Interpolation aka f'strings'
-------------------------------------------

If security isn't a concern and you only have to support 3.6 or later, you should use **f'strings'**.

>>> f'Hello, {name}!'
'Hello, Mark!'

They are pretty, and they are powerful:

>>> a = 5
>>> b = 10
>>> f'Five plus ten is {a + b} and not {2 * (a + b)}.'
'Five plus ten is 15 and not 30.'


New-style formatting
--------------------

If you have to support 2.7 - 3.5, use new-style formatting.

>>> 'Hello, {}'.format(name)
'Hello, Mark'

They aren't pretty, but they work in every case imaginable and they aren't old-style.

Old-style formatting
--------------------

We should have no code that needs to support 2.6 or older. Therefore, there is never any reason to use old-style.  It's
ugly and hard to read.

>>> 'Hello, %s' % name
'Hello, Mark'

Also, the below is not a valid form of string concatenation.  Never use it.

>>> first = 'Mark'
>>> last = 'Baggett'
>>> first + ' ' + last
'Mark Baggett'




----------
Deployment
----------

Pipenv
======

About
-----

While not everyone in the Libraries writes Python, many people use code that is written by someone else.  For that
reason, we should make our code as easy to deploy as possible.

To keep system Python clean and bring some consistency to virtual environments and deployment, the Libraries use
`Pipenv <https://pipenv.readthedocs.io/en/latest/>`_.

We have helpful instructions elsewhere for getting started.

Packages and Devpackages
------------------------

Pipenv for us is all about deployment.  Therefore, we should try to limit packages to just the things one would need to
execute our code.







Requirements.txt
================


Setup.py
========


-----
Style
-----
