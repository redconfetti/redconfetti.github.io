---
layout: post
title: Objective C Notes
date: '2014-01-09 07:46:44 -0800'
comments: true
categories:
- Mac OS X
tags: []
---

I'm exploring Objective C right now. There are some things that I notice and am
curious about, so I'm going to note what I find here.
<!--more-->

## ArgC and ArgV

Many tutorials will likely start you off using XCode to create a command line
application. The 'main' function with Objective C is defined like so:

``` objective_c
int main(int argc, const char * argv[])

{

    @autoreleasepool {

        // insert code here...

        NSLog(@"Hello, World!");

    }

    return 0;

}
```

I wondered what the two parameters for the main function represent. It turns out
that "argc" means "argument count". It signifies how many arguments are being
passed into the command line tool you are creating. "argv" means "argument
values", and is a pointer to an array of characters, otherwise representing the
string of arguments.

[Objective-c main routine, what is: int argc, const char * argv[]](http://stackoverflow.com/questions/4575801/objective-c-main-routine-what-is-int-argc-const-char-argv)

## Objective C File Extension

I wondered why Objective C files end with '.m' instead of '.oc' or something
like that. The inventor of Objective C, Brad Cox, has indicated it's because .o
and .c were already taken by the C language. It is said that the 'm' stands for
'messages', and that some call them 'method files'.

See [Why do Objective C files use the .m extension?](http://stackoverflow.com/questions/652186/why-do-objective-c-files-use-the-m-extension)
