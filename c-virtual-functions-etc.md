---
layout: post
title: C++ Concepts - Virtual Function, Pure Virtual Function, Abstract class, And Pure Abstract Class
mathjax: false
related: false
comments: true
---

_Created: Feb, 2016_

* Virtual function: a function defined in a base class that should be overridden by the derived classes. The virtual function is implemented in base class, thus can be instantiated together with the base class. Syntax example: 
```
virtual void vfunc(int n);
```

* Pure virtual function: similar to virtual function except that it is without implementation in the base class. Therefore it cannot be instantiated with the base class and thus the base class becomes un-instantiable. Syntax example:
```
pure virtual void vfunc(int n) = 0;
```

* Abstract class: a class having at least one pure virtual function, thus can only be derived but not instantiated. However, an abstract class may have virtual function with implementation. Any derived class still having pure virtual function(s) is still abstract class. Syntax example: 

```
class Human
{
protected:
    std::string name;
 
public:
    Human(std::string name)
        : name(name)
    {
    }
 
    std::string get_name() { return name; }
    virtual void hello() { std::cout << "Hello from a human!" << std::endl; };  // virtual function
    virtual char* speak() = 0;  // pure virtual function
};
```

* Pure abstract class or interface class: an abstract class with pure virtual functions only.

* [A discussion](http://stackoverflow.com/questions/5688865/virtual-functions-and-abstract-classes) on the scenario that an abstract class having (non-pure) virtual functions. Since the an abstract class cannot be instantiated anyway,  the implementation inside virtual functions is useless (essentially like a pure virtual class). 

## Links:

* [This post](http://www.learncpp.com/cpp-tutorial/126-pure-virtual-functions-abstract-base-classes-and-interface-classes/) explains it well. 

* [Abstract classes explained in MSDN](https://msdn.microsoft.com/en-us/library/c8whxhf1.aspx)


