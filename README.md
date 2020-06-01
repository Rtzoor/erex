# erex
Expressive Regular Expressions pre-processor

<p>Regex syntax is cryptic and intimidating at first glance. This project aims to make available a more expressive regex syntax that makes it easier to understand what is happening in a given pattern. every pattern can be verbally explained in a strict manner. </p><p>the goal is to enable the reader to more intuitively understand what the pattern matches.</p>

<p>the syntax is as follows:</p>

* '' = expressive sequence <p>TODO: add explanation</p>

* () = contained sequence <p>TODO: add explanation</p>

* @ = location <p>TODO: add explanation</p>

* ()@{end} = () at end of row <p>TODO: add explanation</p>

* ()@{start} = () at start of row <p>TODO: add explanation</p>

* {any} = any character except line breaks <p>TODO: add explanation</p>

* {anynum} = any number between 0 and 9 <p>TODO: add explanation</p>

* {anylowchar} = any character between a and z <p>TODO: add explanation</p>

* {anyhighchar} = any character between A and Z <p>TODO: add explanation</p>

* {anyfrom}() = any character in () <p>TODO: add explanation</p>

* {anyexcept}() = any character except characters in () <p>TODO: add explanation</p>

* {min0}() = zero or more concurrent occurances of () <p>TODO: add explanation</p>

* {min1}() = one or more concurrent occurances of () <p>TODO: add explanation</p>

* {only}() = entire content of row from start to finish is () <p>TODO: add explanation</p>

<p>the expressive form of the result of a pattern search can be described as such:</p>
<p>rows that contain a sequence of /</p>
<p>where "/" is substitued with the resulting expressive pattern, that can be translated to a normal sentence that describes the result.</p>

<p>examples with grep, courtesy of my linux course instructor, Muhamad Eawedat:</p>
<pre>rows that contain a sequence of /
grep "root" test                / 'root' / rows that contain a sequence of 'root'
grep "r$" test                  / '(r)@{end}' / rows that contain a sequence of 'r' at end of row
grep "^root" test               / '(root)@{start}' / rows that contain a sequence of 'root' at start of row
grep "r..f" test                / 'r{any}{any}f' / rows that contain a sequence of 'r' any character except line breaks twice 'f'
grep "r..t" test                / 'r{any}{any}'t' / rows that contain a sequence of 'r' any character except line breaks twice 't'
grep "...." test                / '{any}{any}{any}{any}' / rows that contain a sequence of any character except line breaks four times
grep "..." test                 / '{any}{any}{any}' / rows that contain a sequence of any character except line breaks three times
grep "[0-9]" test               / '{anyfrom}(0123456789)' / rows that contain a sequence of any number between 0 and 9
grep "[0123456789]" test        / '{anyfrom}(0123456789)' / rows that contain a sequence of any number between 0 and 9
grep "[^0-9]" test              / '{anyexcept}(0123456789)' / rows that contain a sequence of any character except numbers between 0 and 9
grep "[.]" test                 / '.' / rows that contain a sequence of '.'
grep "." test                   / '{any}' / rows that contain a sequence of any character except line breaks
grep "re*d" test                / 'r{min0}(e)d' / rows that contain a sequence of 'r' zero or more concurrent occurances of (e) 'd'
grep "r[oe]*d" test             / 'r{min0}({anyfrom}(oe))d' / rows that contain a sequence of 'r' zero or more concurrent occurances of any character of (oe) 'd'
grep "z*" test                  / '{min0}(z)' / rows that contain a sequence of zero or more concurrent occurances of (z)
grep "ee*" test                 / 'e(e){min0}' / rows that contain a sequence of 'e' zero or more concurrent occurances of (e)
grep "^..j.r$" test             / '{only}({any}{any}j{any}r)' / rows that contain a sequence of any character except line breaks twice 'j' any character except line breaks 'r' and only that sequence is in the row
grep "^123" test                / '(123)@{start}' / rows that contain a sequence of '123' at start of row
grep "^[0-9].*" test            / '({anyfrom}(0123456789){min0}({any}))@{start}' / rows that contain a sequence of any number between 0 and 9 and zero or more concurrent occurances of any character except line breaks
grep "c[aeiou]ll" test          / 'c{anyfrom}(aeiou)ll' / rows that contain a sequence of 'c' any character of (aeiou) 'll'
grep "^[aeiou]" test            / '({anyfrom}(aeiou))@{start}' / rows that contain a sequence of any character of (aeiou) at start of row
grep "^unix" test               / '(unix)@{start}' / rows that contain a sequence of 'unix' at start of row
grep "os$" test                 / '(os)@{end}' / rows that contain a sequence of 'os' at end of row
grep ".*" test                  / '({any}){min0}' / rows that contain a sequence of zero or more concurrent occurances of any character except line breaks
grep "kan..roo" test            / 'kan{any}{any}roo' / rows that contain a sequence of 'kan' any character except line breaks twice 'roo'
grep "acce[np]t" test           / 'acce{anyfrom}(np)t' / rows that contain a sequence of 'acce' any character of (np) 't'
grep "co[^l]a" test             / 'co{anyexcept}(l)a' / rows that contain a sequence of 'co' any character except l 'a'
grep "[FG]oo" test              / '{anyfrom}(FG)oo' / rows that contain a sequence of any character in (FG) 'oo'
grep "[0-9][0-9][0-9]" test     / '{anyfrom}(0123456789){anyfrom}(0123456789){anyfrom}(0123456789)' / rows that contain a sequence of any number between 0 and 9 three times
grep "cat" test                 / 'cat' / rows that contain a sequence of 'cat'
grep "c.t" test                 / 'c{any}t' / rows that contain a sequence of 'c' any character except line breaks 't'
grep ".at" test                 / '{any}at' / rows that contain a sequence of any character except line breaks 'at'
grep "[a-c]at" test             / '{anyfrom}(abc)' / rows that contain a sequence of any character in (abc) 'at'
grep "[0-9][0-9]" test          / '{anyfrom}(0123456789){anyfrom}(0123456789)' / rows that contain a sequence of any number between 0 and 9 twice
grep "c[a-zA-Z0-9]t" test       / 'c{anyfrom}({anylowchar}{anyhighchar}{anynum})t' / rows that contain a sequence of 'c' any character in (any character between a and z any character between A and Z any number between 0 and 9)
grep "^ca" test                 / '(ca)@{start}' / rows that contain a sequence of 'ca' at start of row
grep "ing$" test                / '(ing)@{end}' / rows that contain a sequence of 'ing' at end of row
grep "^cat$" test               / '{only}(cat)' / rows that contain a sequence of 'cat' and only that sequence is in the row</pre>
