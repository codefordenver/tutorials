
{% include pre-defs.html %}

<pre hidden><code class="language-klipse">
  (require '[cljs.test :as test])
</code></pre>


[← Previous: Prelude](prelude.html)

## Programming Functionally

Clojure and ClojureScript encourage you to think functionally. With a bit of
practice, it will become natural to write functions through which data flows,
not just cobble together jabbering objects.

### Fizz-Buzz Folly

Here's an easy exercise, often posed as an interview question (From
[Why Can't Programmers.. Program?](https://blog.codinghorror.com/why-cant-programmers-program/){:target="_blank"}):

> Write a program that prints the numbers from 1 through 16. But for multiples of
> three print "Fizz" instead of the number and for the multiples of five print
> "Buzz". For numbers which are multiples of both three and five print
> "FizzBuzz".

#### What's the usual (procedural) approach?

Lets start with Javascript, a language favoring a procedural, object-oriented
coding style. The exercise starts with "print the numbers from 1 to 16". So
lets reach for a venerable for-loop and print the numbers and the Fizzes. Then ...

- Distinguish between the "Fizz" and "Buzz" cases and print them. <br/>
  Click to see the docs -->
  &nbsp;[`for`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for){:target="_blank"}
  &nbsp;[`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else){:target="_blank"}
  &nbsp;&nbsp;[`%`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Remainder){:target="_blank"}
  &nbsp;&nbsp;[`===`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Identity){:target="_blank"}
  &nbsp;[`console.log`](https://developer.mozilla.org/en-US/docs/Web/API/Console/log){:target="_blank"}
- But now what about the "FizzBuzz" case?
 
```klipse-es2017
for ( let i = 1; i <= 16; i++ ) {
  if ( i % 3 === 0 ) {
    console.log("Fizz")
  } else {
    console.log(i);
  }
}
```

So we're generating the desired output, but ...

- We're printing to the Console, but what if we need the _sequence_ of values?
- There is duplication. What if "Fizz" should be on the 4's?

```klipse-es2017
for ( let i = 1; i <= 16; i++ ) {
  if ( i % 3 === 0  &&  i % 5 === 0 ) {
    console.log("FizzBuzz")
  } else if ( i % 5 === 0 ) {
    console.log("Buzz")
  } else if ( i % 3 === 0 ) {
    console.log("Fizz")
  } else {
    console.log(i);
  }
}
```

#### The functional approach

Above, our thought process began with the side effects we ultimately wished to
perform. Instead, let's begin with the _data_.

Yes, we want to _print_ a list of numbers, but what if we first think about
the list itself. We can make an infinite list of numbers.

- Click to see the docs -->
  &nbsp;[`range`](http://cljs.github.io/api/cljs.core/range){:target="_blank"}
- Can we make it infinite and still use it?
  &nbsp;[`take`](http://cljs.github.io/api/cljs.core/take){:target="_blank"}
- Can we chop off the first element?
  &nbsp;[`rest`](http://cljs.github.io/api/cljs.core/rest){:target="_blank"}
- Can we make a sequence of results of a function applied to each element?
  &nbsp;[`map`](http://cljs.github.io/api/cljs.core/map){:target="_blank"}

```klipse
(range 17)
```

What about the Fizz's and Buzz's? How about a list where every third item is
"Fizz", and we just make the others empty. The ClojureScript function [`cycle`](http://cljs.github.io/api/cljs.core/cycle){:target="_blank"}
Makes a new _infinite_ list by repeating the given one over and over: 

- Can we combine the elements of the Fizz list with those of the Buzz list
  giving a sequence of
  [`vector`](http://cljs.github.io/api/cljs.core/vector){:target="_blank"}s?
  &nbsp;[`map`](http://cljs.github.io/api/cljs.core/map){:target="_blank"}
- Then also combine the list of numbers.

```klipse
(take 16 (cycle ["" "" "Fizz"]))
```

Now try out a custom function, `pick-fz-bz-or-n`, defined below. There are
many ways to test.
&nbsp;[cljs.test](http://cljs.github.io/api/cljs.test/){:target="_blank"}
&nbsp;[is](http://cljs.github.io/api/cljs.test/is){:target="_blank"}
&nbsp;[are](http://cljs.github.io/api/cljs.test/are){:target="_blank"}

```klipse
; Maybe we could use function pick-fz-bz-or-n.
; Uncomment to see its doc-string:
;(my-doc #'pick-fz-bz-or-n)

#_         ; Note that #_(...) is another kind of comment.
(test/are
  [fz     bz     n   result] (= (pick-fz-bz-or-n fz bz n)
                                result)
  ""      ""     16  16
  "Fizz"  ""     3   "Fizz"
  ""      "Buzz" 5   "Buzz"
  "Fizz"  "Buzz" 15  "FizzBuzz"
  )
  
```

Now we have the sequence of numbers and strings we want.

- Can we make the function annonymous for brevity?
  &nbsp;[`fn`](http://cljs.github.io/api/cljs.core/fn){:target="_blank"}
  &nbsp;[`#`](http://cljs.github.io/api/syntax/function){:target="_blank"}
- Can we print each element, one to a line?
  &nbsp;[`prn`](http://cljs.github.io/api/cljs.core/prn){:target="_blank"}

```klipse
(defn pick-fz-bz-or-n
  "Returns the concatenation of the given strings, or if empty
  returns the last argument."
  [fz bz n]
  (-> (str fz bz)
      not-empty
      (or n)))

(take 16 (map pick-fz-bz-or-n
              (cycle ["" "" "Fizz"])
              (cycle ["" "" "" "" "Buzz"])
              (rest (range))))
```

### Fibonacci Cha-Cha

The [Fibonacci sequence](https://en.wikipedia.org/wiki/Fibonacci_number){:target="_blank"}
starts with 0 and 1, then every later number is simply the sum of the previous
two. Let's make a function to generate the nth (zero-based) Fibonacci number.
An ordinary procedural approach might look like this:

```klipse-es2017
function fib(n) {
  const fibs = [0, 1]
  for ( let i = 2; i < n; i++ ) {
    fibs.push( fibs[i - 2] + fibs[i - 1] )
  }
  return fibs
}

fib(16)
```

Again, we started with the _action_ of looping over side effects, in this
case, repeatedly mutating the array `fibs`.

Instead, let's turn the "action" inside-out by starting with the _data_. We
can define the data itself recursively, since the elements past the first two
depend entirely on the two previous ones.

```klipse
(def fibs
  (lazy-cat [0 1] (map + fibs (rest fibs))))

(test/is (= (nth fibs 10) 55))

(take 16 fibs)
```

Notice how we haven't instructed the computer to _do_ something. We've just
_declared_ what the data must look like. Where did the loop go?

---

[Next: Conclusion →](conclusion.html)


{% include klipse.html %}

