# Raise … from … in Python – Stefan Scherfke

markdownload-timestamp:: 2023-04-14T08:51:29 (UTC -05:00)
markdownload-source:: https://stefan.sofa-rockers.org/2020/10/28/raise-from/
markdownload-hostname:: stefan.sofa-rockers.org
author:: 
tags:: Python, Exception Handling



## Excerpt
> When you recently upgraded to pylint 2.6.0, you may have stumbled
across a new warning:

---
When you recently upgraded to [pylint 2.6.0](https://pypi.org/project/pylint/2.6.0/), you may have stumbled across a new warning:

```
src/mylib/core.py:74:20: W0707:
  Consider explicitly re-raising using the
  'from' keyword (raise-missing-from)
```

The reason for this message is an exception that you raised from within an `except` block like this:

```
>>> class MyLibError(Exception):
...     """Base class for all errors raised by mylib"""
...
>>> def do_stuff("onoes"):
...     try:
...         int(text)
...     except ValueError as e:
...         raise MyLibError(e)

```

When you run `do_stuff()`, you’ll get the following traceback:

```
>>> do_stuff(text)
Traceback (most recent call last):
  File "<stdin>", line 3, in do_stuff
ValueError: invalid literal for int() with base 10: 'onoes'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 5, in do_stuff
__main__.MyLibError: invalid literal for int() with base 10: 'onoes'

```

The important line here is:

> During handling of the above exception, another exception occurred

This means that while you were handling the `ValueError`, another (unexpected) exception occurred: a `MyLibError`.

But this is not what we wanted to do – we wanted to _replace_ the `ValueError` with `MyLibError`, so that our uses only have to handle a single exception type!

## Enter `raise … from …`

To express _I want to modify and forward an existing exception_, you can use the `raise _NewException_ from _cause_` syntax:

```
>>> def do_stuff(text):
...     try:
...         int(text)
...     except ValueError as e:
...         raise MyLibError(e) from e

```

When we run this now, we’ll get a different traceback printed:

```
>>> do_stuff("onoes")
Traceback (most recent call last):
  File "<stdin>", line 3, in do_stuff
ValueError: invalid literal for int() with base 10: 'onoes'

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 5, in do_stuff
__main__.MyLibError: invalid literal for int() with base 10: 'onoes'

```

Your users will now receive a `MyLibError` with the attached information, that the cause of this error was a `ValueError` somewhere in your code.

## When the underlying cause is not important

If your users shouldn’t care about the underlying cause, because the new exception contains all the relevant information (i.e., that the provided input cannot be parsed), you may also omit the cause:

```
>>> def do_stuff(text):
...     try:
...         int(text)
...     except ValueError as e:
...         raise MyLibError(e) from None

```

When you run this now, you’ll get a nice an clean traceback:

```
>>> do_stuff("onoes")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 5, in do_stuff
__main__.MyLibError: invalid literal for int() with base 10: 'onoes'

```

## Summary

When you raise an exception from within an `except` block in Python, you have three options:

-   If a new/unexpected exception occurs in the code handling the original exception, `raise _NewException_`.
-   If you want to wrap the original exception(s) (e.g., with a common base exception to reduce complexity for your users), `raise _NewException_ from _cause_`.
-   If you want to hide the original exception because it is irrelevant for your users, `raise _NewException_ from None`.
