# RASP_solutions


## Homework Solutions

**Problem 1:**
Write an s-op that contains the indices in reverse order.
For example:
```
> reverse_indices("hello")
[4, 3, 2, 1, 0]
```

**Solution 1:**
```
>> def reverse_function(seq) {
..             return aggregate(select(indices, length-indices-1, ==), seq);
..         }   
     console function: reverse_function(seq)
>> reverse = reverse_function(indices);
     s-op: reverse
     Example: reverse("hello") = [4, 3, 2, 1, 0] (ints)
>> reverse("hello");
     =  [4, 3, 2, 1, 0] (ints)
```

**Problem 2:**
Write a function that "rotates" the input text by the specified number of characters.
For example:
```
> rotate(tokens, 0)("hello")
"hello"
> rotate(tokens, 1)("hello")
"ohell"
> rotate(tokens, 2)("hello")
"lohel"
```

**Solution 2:**
```
>> def rotate(seq,count){
..   return aggregate(select(indices, indices-count, ==),seq,"a") if indices >= count else aggregate(select(indices, indices+length-count, ==),seq,"a");
..   }
     console function: rotate(seq, count)
>> rotate(tokens, 0)("hello");
     =  [h, e, l, l, o] (strings)
>> rotate(tokens, 1)("hello");
     =  [o, h, e, l, l] (strings)
>> rotate(tokens, 2)("hello");
     =  [l, o, h, e, l] (strings)
```

**Problem 3:**
Write a function that takes a sequence as input and "swaps every letter with its neighbor".
Specifically, for every even index $i$, positions $i$ and $i+1$ will be swapped;
if the length of the sequence is odd, then the last element should not move.
For example:
```
> swap(tokens)("hello")
"ehllo"
> swap(tokens)("ababab")
"bababa"
```

**Solution 3:**
```
>> def swap(seq){
..   return aggregate(select(indices, indices+1, ==),seq,"a") if indices%2==0 else aggregate(select(indices, indices-1, ==),seq,"a");
..   }
     console function: swap(seq)
>> def swap_help(seq){
..   return aggregate(select(indices, indices, ==),seq,"a") if length%2==1 and length-indices==1 else swap(tokens);
..   }
     console function: swap_help(seq)
>> swap_help(tokens)("hello");
     =  [e, h, l, l, o] (strings)
>> swap_help(tokens)("ababab");
     =  [b, a, b, a, b, a] (strings)
```

**Problem 4:**
Write a function that returns the maximum value in the sequence repeated for every position.
For example:
```
> maxseq(tokens)("ababcabab")
"ccccccccc"
```

**Solution 4:**
```
>> set example "ababcabab"
>> sorted = sort(tokens,tokens);
     s-op: sorted
     Example: sorted("ababcabab") = [a, a, a, a, b, b, b, b, c] (strings)
>> last_digit = length - 1
.. ;
     s-op: last_digit
     Example: last_digit("ababcabab") = [8]*9 (ints)
>> set example "ababcabab"
>> sorted = sort(tokens,tokens);
     s-op: sorted
     Example: sorted("ababcabab") = [a, a, a, a, b, b, b, b, c] (strings)
>> last_digit = length - 1;
     s-op: last_digit
     Example: last_digit("ababcabab") = [8]*9 (ints)
>> def maxseq(seq){
..   return load_from_location(sorted,last_digit);
..   }
     console function: maxseq(seq)
>> maxseq(tokens);
     s-op: out
     Example: out("ababcabab") = [c]*9 (strings)
```

**Problem 5:**
Write a function that returns the maximum value in the sequnce only of the tokens seen so far.
For example:
```
> maxseq(tokens)("ababcabab")
"abbbccccc"
```
> *Hint:*
> The `mask_ag` selector is useful for any problem that is either "autogenerative" or using only tokens "seen so far".

**Problem 6:**
Write a function that performs sequence reversal "autogeneratively".
That is, it will take a sequence as input, and the sequence will contain a special token `$` that marks the "end of the prompt".
The text before the `$` should be unchanged, and the text after the `$` should be the text before the `$` reversed (this text represents the model's response to the prompt).
The code should be robuse to the case when the length of text after `$` is not the same as the length of text before `$`.
For example:
```
> reverse_ag(tokens)("hello$     ")
"hello$olleh"
> reverse_ag(tokens)("hello$ ")
"hello$o"
> reverse_ag(tokens)("hello$X")
"hello$o"
> reverse_ag(tokens)("hello$XXXXXXXXXX")
"hello$olleh     "
```

**Problem 6:**
Write a function that counts the number of times a certain token appears in the input sequence.
For example:
```
> howmany(tokens, "a")("hello")
"00000"
> howmany(tokens, "h")("hello")
"11111"
> howmany(tokens, "l")("hello")
"22222"
```

**Problem 7:**
Write a function that counts the number of times a certain token has appeared in the input sequence so far.
For example:
```
> howmany(tokens, "a")("hello")
"00000"
> howmany(tokens, "h")("hello")
"11111"
> howmany(tokens, "e")("hello")
"01111"
> howmany(tokens, "l")("hello")
"00122"
```
**Problem 8:**
Create your own "interesting" problem statement.
Write a function/s-op that solves this problem.

