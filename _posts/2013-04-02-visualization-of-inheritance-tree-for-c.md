---
layout: post
title: "Inheritance Hierarchy Visualization for C++"
description: ""
category: Python
tags: [clang, C++, python]
---
{% include JB/setup %}

![Inheritance Tree] (/assets/images/2013-04-02-inheritance-tree.png "Inheritance tree demo")

## Digesting Large C++ Code Base
A few weeks ago, a friend of mine was twittering about the trouble of getting familiar with a new large C++ code base. He was complaining about that the class relation is simply too complex to digest; the inheritance relation goes so deep that he has to open a spreadsheet to manage it.

Then I asked, "Would it be nice if we have a piece of software that can automatically generate the inheritance graph for me?"

Several days later, I saw a [post] (http://stackoverflow.com/questions/1271513/c-code-visualization) at stackoverflow asking about c++ source code visualization tool that inspires me to write a little tool to do that for me (even though there may be already existing software capable of doing this) and I can be familiar with [libclang] (http://clang.llvm.org/doxygen/group__CINDEX.html) which I wanted to study for a while.

It turns out, it does not require me to write too many lines of code to achieve that goal; the following python codes will mainly do the job.

{% highlight python %}
import os
from clang.cindex import TranslationUnit
from clang.cindex import TokenKind, CursorKind
from clang.cindex import Index
 
kInputsDir = os.path.join(os.path.dirname(__file__), 'cpp')
 
def each_class_cursor(cursor):
    for c in cursor.get_children():
	if c.kind == CursorKind.CLASS_DECL:
	    yield c
	for cls in each_class_cursor(c):
	    yield cls
 
def each_inheritance_relation(cursor):
    for cls in each_class_cursor(cursor):
	for c in cls.get_children():
	    if c.kind == CursorKind.CXX_BASE_SPECIFIER:
		yield cls.displayname, c.get_definition().displayname
 
if __name__ == '__main__':
    # demo parsing the base class and parent class relations
    cpp_file_path = os.path.join(kInputsDir, 'main.cpp')
    index = Index.create()
    tu = index.parse(cpp_file_path)
    for this, parent in each_inheritance_relation(tu.cursor):
	print this, parent
{% endhighlight %}


each_inheritance_relation here will iterate the base and derived class relation so that the remaining manipulation such as plotting the graph can be post-processed using [pygraphviz] (http://networkx.lanl.gov/pygraphviz/), for example.

Or, you can clone my [little tool] (https://github.com/shaochuan/clang-tools) to play with it. The little trick of making clang parse the codes correctly is that you have to tell clang the include path correctly. Otherwise, it cannot find the correct declaration and won't produce the correct result. 

Specifically,

{% highlight python %}
index = Index.create()
# extra_args = ['-stdlib=libc++', '-std=c++0x']
# include_args = ['-I/path/to/include']
tu = index.parse(cpp_file_path, args=extra_args+include_args)
{% endhighlight %}

The extra 'args' arguments will be passed to clang, and [here] (http://clang.llvm.org/doxygen/group__CINDEX__TRANSLATION__UNIT.html#ga2baf83f8c3299788234c8bce55e4472e) has more details about it. Or you can type

{% highlight python %}
help(index.parse)
{% endhighlight %}

to see how the documentation explains about 'args'.

In addition, to make the codes run on your machine successfully, you need to have [clang] (http://clang.llvm.org/) installed on your machine. If you are using Mac OS 10.7.x and have XCode 4.5 on the machine, your system already have clang with XCode.

Now, all you need to do is to clone its [python ctypes binding] (https://github.com/llvm-mirror/clang/tree/master/bindings/python), and add a library path in your .bash_profile,

{% highlight bash %}
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib
{% endhighlight %}

If you get the following error,

	clang.cindex.LibclangError: dlopen(libclang.dylib, 6): image not found. To provide a path to libclang use Config.set_library_path() or Config.set_library_file().

That means your LD_LIBRARY_PATH was not set up correctly. Make sure the LD_LIBRARY_PATH contains the file libclang.dylib, for example.

	$ ls /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib
	arc                 clang               libclang.dylib      libprofile_rt.dylib
	c++                 libLTO.dylib        libprofile_rt.a

## Plotting Class Hierarchy

Once we have the above python module, we can easily plot the class hierarchy using [pygraphviz] (http://networkx.lanl.gov/pygraphviz/).

{% highlight python %}
import os
import pygraphviz
import inherit_relation as ir
from clang.cindex import Index

kInputsDir = os.path.join(os.path.dirname(__file__), 'cpp')

def draw(G, output_filename):
    G.layout(prog='dot')
    G.draw(output_filename)

def build():
    cpp_file_path = os.path.join(kInputsDir, 'main.cpp')
    index = Index.create()
    tu = index.parse(cpp_file_path)
    G = pygraphviz.AGraph(directed=True)
    for this, parent in ir.each_inheritance_relation(tu.cursor):
        edge = (this, parent)
        G.add_edge(edge)
    return G

if __name__ == '__main__':
    graph = build()
    draw(graph, 'inherit.png')
{% endhighlight %}

build() function is building directed graph containing the class inheritance relation and draw() function will dump the graph into a PNG file.

Now if you clone my [repo] (https://github.com/shaochuan/clang-tools), you can simply run the following script to plot the exact graph in this blog post.

	$ python simple_inherit_graph_demo.py

