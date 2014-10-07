---
layout: post
title: Off to see the wizard
---

Along with some fellow [Hacker Schoolers](http://www.hackerschool.com), I'm about to embark on the journey of working through [Structure and Interpretation of Computer Programs](http://mitpress.mit.edu/sicp/) or SICP as it's affectionately known. The examples in the book are implemented in a dialect of [Lisp](http://en.wikipedia.org/wiki/Lisp_(programming_language)) called  [Scheme](http://en.wikipedia.org/wiki/Scheme_(programming_language)). Much like SICP itself, I've long wanted to learn Lisp so I'm looking forward to working through the exercises. Before I do, however, I needed to get my environment in order. Doing so took some digging so I thought I'd share my experience for other Ubuntu folk tredding the same path.

# REPL the hard way

I'm not sure which language I used where I came across REPL but it seems to be an article of faith that it's the best way to get started learning a language. As someone who was very accustomed to using IDEs in development, I was initially very reluctant. However, after the initial resistance I found that I rather enjoyed it and found the immediate gratification it provides pretty addictive. 

I've mentioned that SICP uses Scheme but it's important to note that it's the code in SICP is implemented using a particular implementation: [MIT Scheme](http://en.wikipedia.org/wiki/MIT/GNU_Scheme). A quick search revelated that a package exists for MIT Scheme so I fired off `sudo apt-get install mit-scheme` in my shell. As with many things in life that should be simple but are not, this ended up in an error message. After some digging it seems that a 64-bit version of the MIT Scheme package is not available so apt was attempting to install a 32-bit version which shouldn't be a problem but failed due to a dependency on a 32-bit library (specifically, `libmcrypt4:i386`).

I was able to get around this by running `sudo apt-get install libmcrypt4:i386` and again `sudo apt-get install mit-scheme`. Finally, I was able to invoke `mit-scheme` successfully and, in the words of Eddie Izzard, "et voilà":

```
$ mit-scheme
```

```
MIT/GNU Scheme running under GNU/Linux
Type '^C' (control-C) followed by 'H' to obtain information about interrupts.
```

```
Copyright (C) 2011 Massachusetts Institute of Technology
This is free software; see the source for copying conditions. There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

```
Image saved on Tuesday October 22, 2013 at 12:31:09 PM
  Release 9.1.1 || Microcode 15.3 || Runtime 15.7 || SF 4.41 || LIAR/i386 4.118
  Edwin 3.116
```

```
1 ]=>
```

# A simpler path?

If you eschew the use of 32-bit binaries and a single apt-get call seems too much already, let me introduce you to DrRacket. I thought that the Java ecosystem had cornered the market on introducing tiresoome and arguably  clever variations (in the case of Java, on one bad coffee reference after another) when it comes to practice of naming projects and products but it turns out that it's only a more recent case of a long-standing condition. Some might say that is typical of Java in a number of ways but I'll leave that to others to debate.

At any rate, I found that Scheme led to [Racket](http://en.wikipedia.org/wiki/Racket_(programming_language)) which is a descendant of Scheme. [DrRacket](http://docs.racket-lang.org/drracket/) is an IDE designed to implement programs in Racket. I found that it's possible to use DrRacket in a special mode that supports MIT Scheme and a [post](http://www.neilvandyke.org/racket-sicp/) that describes using DrRacket for the very purpose of working through SICP. This was all good and well but DrRacket has an UI component that I find distracting. Fortunately, I came across an excellent [post](http://crash.net.nz/posts/2014/08/configuring-vim-for-sicp/) which details how to run DrRacket in a mode that allows for REPL-like functionality in a terminal while also supporting the MIT Scheme mode. 

    $ racket -i -p neil/sicp -l xrepl
    Welcome to Racket v5.3.6.
    -> 

As the author of the last-referenced post mentions, there are a number of other alternatives that could be used aside from the two I've discussed here. He clearly prefers the DrRacket REPL and looks askance on the MIT Scheme REPL. Despite his dislike for it, I'm inclined to start with it and see how far it takes me.
