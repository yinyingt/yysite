---
layout: post
title: Linking and Library Sharing with GCC Explained by Examples
mathjax: true
related: false
comments: true
---

_Last modified: Jun, 2014_

Compiling and assembling convert the high-level source code (such as written in C programming language) to objective code (machine code). However, to make the program executable, external libraries called in the source code have to be included. The process of linking stitches those pieces together and make the final executable. Compiler like GNU Compiler Collection (GCC) can do compiling, assembling and linking all together. 

There are generally two types of linking: static linking and dynamic linking. The former links objective code in the compiling time, while the latter does it in run time. The difference will be made clear by examples in the following. Linux environment is assumed. 


## A Baisc Example

The example source code for demonstration purpose is as follows: 

* The working directory is "$HOME/demo". 

* Under the working directory, file "test.c" has the content

{% highlight C %}
#include <blob1.h>

int main()  {
    print_hello();
    return 0; 
}
{% endhighlight %}

* Under "$HOME/demo/my_headers", there is "blob1.h" with the content

{% highlight C %}
void print_hello(void);
{% endhighlight %}

and "blob1.c" with the content

{% highlight C %}
#include <stdio.h>
#include "blob1.h"

void print_hello(void) {
        int n = 1;
        printf("Hello world from blob %i\n", n);
}
{% endhighlight %}

The file hierachy is like 

```
--demo/
  \-- test.c
   -- my_headers/
     \-- blob1.h
      -- blob1.c
```

It is obvious that test.c calls a function "print_hello" in "blob1.c", and "blob.h" is the header file of "blob.c". Note that "test.c" uses brackets rather than double quotes to include "blob.h". 


## Static Compiling

Building an executable from the above files is a two-step process

* 1. Compile "blob.c" without linking since it is a library (use "-c" flag in GCC): 

{% highlight bash%}
cd $HOME/demo/my_headers
gcc blob1.c -c -o blob1.o
{% endhighlight %}

which generates the objective code "blob1.o" under "$HOME/demo/my_headers". 

* 2. Compile "test.c" and link

{% highlight bash%}
cd $HOME/demo
gcc test.c -I./my_headers -c -o test.o 
gcc test.o ./my_headers/blob1.o -o test
{% endhighlight %}

In the above code, "-I" flag specifies the custom include path (./my_headers) other than the default "**/usr/include**" and "**/usr/local/include**" in Linux. The two-line compiling commands can also be consolidated into one line: 

{% highlight bash%}
gcc test.c ./my_headers/blob1.o -I./my_headers -o test
{% endhighlight %}

Testing the executable with

{% highlight bash%}
./test
{% endhighlight %}

will print out 

{% highlight bash%}
Hello world from blob 1
{% endhighlight %}


## Share Static Libraries with Archive

If there are many files in the library, such as "blob1.c", "blob2.c", ..., sharing with an archive can be convenient. Taking the example above, an archive can be created like 

{% highlight bash%}
cd $HOME/demo/my_headers
ar -rcs libmyblob.a blob1.o
mkdir ../my_libs 
mv libmyblob.a ../my_libs
{% endhighlight %}

Note that the archived library bundle should always be named like "lib<name>.a" like "libmystuff.a" here, which is a convention followed by GCC. Now the file hierachy is like this: 

```
--demo/
  \-- test.c
  \-- my_headers/
     \-- blob1.h
     \-- blob1.c
     \-- blob1.o
  \-- my_libs/
     \-- libmystuff.a
```

where the new bundled library archive is placed in another directory other than the previous "my_headers". 

Building an executable is also a two-step process: 

{% highlight bash%}
cd $HOME/demo
gcc test.c -I./my_headers -c -o test.o 
gcc test.o -L./my_libs -lmyblob -o test
{% endhighlight %}

The "-L" flag specifies the library path, and "-l" flag specifies the library name according to the naming convention of library archives. Again the one-liner version is 

{% highlight bash%}
gcc test.c -I./my_headers -L./my_libs -lmyblob -o test.o 
{% endhighlight %}

It can be seen that the header files and library files can be placed in different directories. The process still works if "blob1.c" and "blob1.o" in "$HOME/demo/my_headers" are removed. 


## Dynamic Linking

Dynamic linking links the objective code in run time. The advantage is that different modules of an executable can be updated separately as long as the interfaces among them don't change. In Linux, those dynamic libraries are usually in "**/usr/lib**", "**/usr/local/lib**", etc. Taking the example above, there are extra steps: 

* First, compile the library with position-independent-code (PIC) flag 

{% highlight bash%}
cd $HOME/demo/my_headers
mkdir my_dyn_libs
cd my_dyn_libs
gcc ../my_headers/blob1.c -fPIC -c -o blob1.o
{% endhighlight %}

* Second, compile dynamic library

{% highlight bash%}
gcc blob1.o -shared -o libmyblob.so
{% endhighlight %}

The flag "-shared" means compiling a dynamic library. The naming rule of a dynamic library is "lib<name>.so". Note that the dynamic library also stores the file name inside. **To rename a dynamic library, one needs to recompile with a different name**. 

Now the file hierachy is 

```
--demo/
  \-- test.c
  \-- my_headers/
     \-- blob1.h
     \-- blob1.c
     \-- blob1.o
  \-- my_libs/
     \-- libmyblob.a
  \-- my_dyn_libs/
     \-- libmyblob.so
```

* Finally, build the executable as the static linking example

{% highlight bash%}
cd $HOME/demo
gcc test.c -I./my_headers -L./my_dyn_libs -lmyblob -o test
{% endhighlight %}

To run the executable in Linux, since the dynamic library "libmyblob.so" is not in the default paths (e.g. /usr/lib; /usr/local/lib), environmental variables need to be updated

{% highlight bash%}
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HOME/demo/my_dyn_libs
./test
{% endhighlight %}

Probably it is just easier to place the dynamic library in one of the default paths. 


## Summary

* It is very common to have header files and libraries in different folders. 

* Several GCC flags are reviewed: "-c", "-I", "-L", "l", "-fPIC", "-shared". 

* The difference between static linking and dynamic linking is demonstrated with an example. 




