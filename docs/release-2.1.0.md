Python-Markdown 2.1.0-Beta Release Notes
========================================

We are pleased to release Python-Markdown 2.1-Beta which makes many 
improvements on 2.0. In fact, we consider 2.1 to be what 2.0 should have been. 
While 2.1 consists mostly of bug fixes, bringing Python-Markdown more inline 
with other implementations, some internal improvements were made to the parser, 
a few new builtin extensions were added, and HTML5 support was added.

Please be aware that Python-Markdown 2.1-Beta is *beta* software and is not
considered production ready pending the release of 2.1-Final.

Python-Markdown supports Python versions 2.4, 2.5, 2.6, 2.7, 3.1, and 3.2 out 
of the box. In fact, the same codebase installs on Python 3.1 and 3.2 with no 
extra work by the end user.

Backwards-incompatible Changes
------------------------------

While Python-Markdown has received only minor internal changes since the last
release, there are a few backward-incompatible changes to note:

* Support had been dropped for Python 2.3. No guarantees are made that the 
library will work in any version of Python lower than 2.4. Additionally, while 
the library had been tested with Python 2.4, consider Python 2.4 support to be 
depreciated. It is not likely that any future versions will continue to support
any version of Python less than 2.5. Note that Python 3.0 is not supported due 
to a bug in its 2to3 tool. If you must use Python-Markdown with Python 3.0, it 
is suggested you manually use Python 3.1's 2to3 tool to do a conversion.

* Python-Markdown previously accepted positional arguments on its class and
wrapper methods. It now expects keyword arguments. Currently, the positional
arguments should continue to work, but the solution feels hacky and may be 
removed in a future version. All users are encouraged to use keyword arguments 
as documented in [Using Markdown as a Python Library](using_as_module.html).

* Past versions of Python-Markdown provided module level Global variables which
controlled the behavior of a few different aspects of the parser. Those global
variables have been replaced with attributes on the Markdown class. 
Additionally, those attributes are settable as keyword arguments when 
initializing a class instance. Therefore, if you were editing the global 
variables (either by editing the source or by overriding them in your code), 
you should now set them on the class. See 
[Using Markdown as a Python Library](using_as_module.html) for the options 
available.

* If you have been using the [HeaderID](extensions/header_id.html) extension 
to define custom ids on headers, you will want to switch to using the new 
[Attribute List](extensions/attr_list.html) extension. The HeaderId extension 
now only auto-generates ids on headers which have not already had ids defined. 
Note that the [Extra](extensions/extra.html) extension has been switched to use 
Attribute Lists instead of HeaderId as it did previously.

* Some code was moved into the `markdown.util` namespace which was previously
in the `markdown` namespace. Extension authors may need to adjust a few
import statements in their extensions to work with the changes.

* The commandline script name was changed to `markdown_py`. The previous name
(`markdown`) was conflicting with people (and Linux package systems) who also
had markdown.pl installed on there system as markdown.pl's commandline script
was also named `markdown`. Be aware that installing Python-Markdown 2.1
will not remove the old versions of the script with different names. You
may want to remove them yourself as they are unlikely to work properly.

What's New in Python-Markdown 2.1
---------------------------------

Three new extensions were added. [Attribute Lists](extensions/attr_list.html), 
which was inspired by Maruku's feature of the same name, 
[Newline to Break](extensions/nl2br.html), which was inspired by Github 
Flavored Markdown, and [Smart Strong](extensions/smart_strong.html), which 
fills a hole in the Extra extension.

HTML5 is now supported. All this really means is that new block level elements 
introduced in the HTML5 spec are now properly recognized as raw HTML. As
valid  HTML5 can consist of either HTML4 or XHTML1, there is no need to add a
new HTML5  searializers. That said, `html5` and `xhtml5` have been added as 
aliases of the `html4` and `xhtml1` searializers respectively.

An XHTML searializer has been added. Previously, ElementTree's XML searializer 
was being used for XHTML output. With the new searliazer we are able to avoid
more invalid output like empty elements (i.e., `<p />`) which can choke 
browsers.

Improved support for Python 3.x. Now when running `setupy.py install` in 
Python 3.1 or greater the 2to3 tool is run automatically. Note that Python 3.0 
is not supported due to a bug in its 2to3 tool. If you must use Python-Markdown
with Python 3.0, it is suggested you manually use Python 3.1's 2to3 tool to
do a conversion.

Methods on instances of the Markdown class that do not return results can now
be changed allowing one to do `md.reset().convert(moretext)`.

The Markdown class was refactored so that a subclass could define it's own 
`build_parser` method which would build a completely different parser. In
other words, one could use the basic machinery in the markdown library to 
build a parser of a different markup language without the overhead of building
the markdown parser and throwing it away.

Import statements within markdown have been improved so that third party 
libraries can embed the markdown library if they desire (licencing permitting).

Added support for Python's `-m` command line option. You can run the markdown 
package as a command line script. Do `python -m markdown [options] [args]`. 
Note that this is only fully supported in Python 2.7+. Python 2.5 & 2.6 
require you to call the module directly (`markdown.__main__`) rather than
the package (`markdown`). This does not work in Python 2.4.

The commandline script has been renamed to `markdown_py` which avoids all the 
various problems we had with previous names.  Also improved the commandline 
script to accept input on stdin.

The testing framework has been completely rebuilt using the Nose testing 
framework. This provides a number of benefits including the ability to better
test the builtin extensions and other options available to change the parsing 
behavior. See the [Test Suite](test_suite.html) documentation for details.

Various bug fixes have been made, which are too numerous to list here. See the 
[commit log](https://github.com/waylan/Python-Markdown/commits/master) for a 
complete history of the changes.
