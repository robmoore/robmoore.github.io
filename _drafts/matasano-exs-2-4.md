---
layout: post
title: Matasano Challenge Set 1, Challenges 2, 3, and 4
---
## Fixed XOR

In the process of implementing [challenge 1](http://cryptopals.com/sets/1/challenges/1/), one of my [classmates](http://www.cdosborn.com) who happened to be doing the same challenege also in Scheme (we're both working our way through SICP so it's not too suprising but it a happy
coincidence nonetheless) mentioned that he'd used bit strings in Scheme in his solution. I wasn't aware of bit strings and had used an approach
of simply constructing binary strings using ```string->number```. The [Scheme documentation](https://groups.csail.mit.edu/mac/ftpdir/scheme-7.5/7.5.17/doc-html/scheme_10.html) mentions that character strings are actually a better choice from a performance perspective.
Accordingly, I decided to stick with my initial approach of storing these binary values as strings but did run into cases
where it wasn't pretty. For example, as I concatenated together these strings leading zeros were important and necessitated
the use of zero padding in cases where ```number->string``` dropped the leading zero. This extra processing alone
probably nixed any performance advantages gained from using strings instead of bit strings but stubborness won out.

For this [challenge](http://cryptopals.com/sets/1/challenges/2/), I decided to make the switch to bit-strings. I found them 
easier to work with generally and felt less conflicted than I did using the string approach. Nevertheless, I did hit some 
snags with bit strings. In particular, the ```bit-string-append``` function had a behavior that was counter intuitive in
that the bits copied from the first argument are less significant then the second so I had to reverse the order of them to 
preserve my bit-string values approriately.

In terms of the code itself, it seemed clear that I needed to use ```bit-string-xor``` to perform the operation but I was
surprised that I needed to explicitly match the length of the bit strings in order for the operation to work correctly. Also,
I had a hunch that the xor operation would not be performed across the entire contents of the incoming input and fortunately
[wikipedia](http://en.wikipedia.org/wiki/XOR_cipher) came to the rescue with an example that enabled me to debug my code and
find where I was off in my implementation. 

## Single-byte XOR cipher

[Challenge 3](http://cryptopals.com/sets/1/challenges/3/) provided a bit more interesting as no solution was provided 
in the problem definition and while a substantial clue was provided, the verification of the solution proved to be as
interesting as the crypto-related code itself. I was introduced to [ETAOIN SHRDLU](http://en.wikipedia.org/wiki/Etaoin_shrdlu) which reminded me of when I first came across [Lorem Ipsum](http://en.wikipedia.org/wiki/Lorem_ipsum) years ago. It seems the world of
typesetting or printing was comprised of a lot of practical pranksters.

I found this challenge to feel the most like actually doing crypto-like work possibly in part to the solution not being
readily available.

I've really enjoyed implementing these solutions in Scheme. It's provided me with a lot of insight into the language and
a familiarity with the functions available within the language. I'm somewhat surprised with how accustom I've come to
writing code in Scheme. It's been hard to break some habits from years of Java (for example, my muscle memory really
comes through when it comes to infix notation). Scheme is perhaps too flexible in this regard and I've found a number
of times where I had entered an invalid expression ```(3 * 3)``` and received no feedback from the interpreter that a problem
exists. With great flexibility, I suppose...

I decided that rather than commit the solution with the decoded text, I would create a function to encode the message 
using another 'key' (that is, a different character). This proved very satisfying as I could do the full loop of
encoding and decoding. Possibly this will come in handy further into the challenge set.

## Detect single-character XOR

[Challenge 4](http://cryptopals.com/sets/1/challenges/4/) was really just a variation on the last challenge. I decided
to simply use the same strategy to find a possible key for each string using the result with the highest frequency
of common letters in the English language and then to select the one amongst these with the highest overall score.

Given that there are 327 strings to check using 128 keys, the computation time is significantly greater than any of
the challenges thus far. In terms of optimizing this approach, I suppose one could use some threshold value and stop
searching once that's reached. The string that won out in my testing garnered a 73.3% rating but the example in
the last challenge result only in 67.6% so it's hard to say which threshold value might reliably work.

{% highlight scheme %}
{% github_sample /robmoore/matasano/blob/master/challenge-4.scm %}
{% endhighlight %}