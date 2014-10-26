---
layout: post
title: Matasano Challenge Set 1, Challenge 1
---
## Hex to Base 64

A number of fellow [Hacker Schoolers](http://www.hackerschool.com) have worked on the Matasano Crypto Challenge. 
It sounded intriguing so I looked at the [first of the challenges](http://cryptopals.com/sets/1/challenges/1/) 
which involves transforming a hex string into a Base64-encoded string. I've been working a bit with Scheme as 
part of my [Structure and Interpretation of Computer Programs](http://sarabander.github.io/sicp/) studies so I 
thought it might be a good way to become better acquainted with the language. 

It wasn't clear to me whether the challenge was to implement a Base64 encoder or to utilize one within my
language of choice. I decide that it might be beneficial to implement it from scratch and dove in. This decision 
was aided by my failure to find anything in Scheme that supports Base64 encoding (at least natively). That's not 
quite accurate as later in the game I did come across a [MIME codec implementation in Scheme](https://github.com/tali713/mitscheme/blob/master/src/runtime/mime-codec.scm) which provides a Base64 encoding function. It's 
an impressive piece of code using features of Scheme that are yet beyond my current understanding of the language. 
Nevertheless, I felt that implementing it from scratch would be good fun and stuck to my plan.

This decision immediately started paying dividends as it prompted me to learn the implementation details of 
[Base64](http://en.wikipedia.org/wiki/Base64). I found a post entitled [Convert Base 64 encoded data to ASCII Text](http://www.hcidata.info/base64.htm) which was particularly helpful and I used this primarily to guide my implementation.

The implementation in Scheme was aided by some code [code](http://schemecookbook.org/Cookbook/StringRecipeDecodingBinaryMessages)
I found which provides some examples of translating the hex to binary and binary to decimal values. 

## Implementation

### Helper functions

I found it useful to implement some helper functions. One set augments the provided functionality of string-head and
string-tail to handle cases where a request would exceed a valid index value by returning what's availavble rather than 
to trigger an error. The other function splits the strings into lists based on a given index value (for example, break 
up the string into a list of 2 character strings). 
{% highlight scheme %}
(safe-string-head "123456" 7)
"123456"

(safe-string-tail "123456" 7)
""

(string-split "123456" 2)
("12" "34" "56")
{% endhighlight %}

### Create a function to translate a hex to a binary string
Map 2 hex characters to 8 bits.
{% highlight scheme %}
(hex-string->binary-string "4D")
"01001101"
{% endhighlight %}
We repeatedly do this for the length of the hex string. 
 
### Create a function to map binary to decimal
This is a little more interesting as we map 24 bits from our
binary string to a set of 4 decimal values by splitting the string into 4 groups of 6-bits each. The decimal value
is derived by mapping the 6 bits to a decimal value.
{% highlight scheme %}
(binary-string->decimals "01001101011000010111001")
(19 22 5 50)
{% endhighlight %}
One useful way I found useful to think about this was to break out the bytes into the 6-bit chunks like so:
{% highlight scheme %}
01100001 01100100  ; broken out by 8
011000 010110 0100 ; broken out by 6, note we are short two bits in this last 'chunk'!
{% endhighlight %}
In the case where we do not have a binary string that is evenly divisible by 6 (for example, one that is 16 bits
in length) so we add zeros to the "fill out" the missing bits (18 [3 * 6] - 16 = 2 bits). So in the case of a 
binary string of 16 digits such as 0110000101100100 we would simply add two additional zeros to the end and treat
it as if it were actually 011000010110010000. While it might seem like we are going to be in trouble when we try 
to decode this value and will end up with the wrong translation, we will acknowledge this later by adding values 
to the output to signal this modification.

### Create a function to translate the decimal values to Base64
Map the decimal value to a table of 64 ASCII characters where 0 equals A, 1 equals B, and so on. 
{% highlight scheme %}
(map decimal->base64 '(19 22 5 50 30 18 1 40 24 22 16))
("T" "W" "F" "y" "e" "S" "B" "o" "Y" "W" "Q")
{% endhighlight %}

### Create a function to pad the Base64 output
Finally, we have a Base64 string but aren't done quite yet. In order to account for those 0s added earlier 
in the translation from a binary string to a decimal value, we record one '=' for each couple of 0s we added. 
So if 2 bits were added for a given chunk, we'd need to add '==' to the end of the generated string. This value
can be derived by using the length of the provided hex string.
{% highlight scheme %}
(base64-encode "4D61727920686164")
"TWFyeSBoYWQ="
{% endhighlight %}

{% gist robmoore/72e1e8fa55b64bd45069 %}
