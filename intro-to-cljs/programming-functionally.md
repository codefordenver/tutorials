
## Programming Functionally

Clojure and ClojureScript encourage you to think functionally. With a bit of
practice, it will become natural to write functions through which data flows,
than to cobble together communicating objects.

### Fizz-Buzz Folly

Here's an easy exercise, often posed as an interview question (From [Why Can't Programmers.. Program?](https://blog.codinghorror.com/why-cant-programmers-program/)):

> Write a program that prints the numbers from 1 to 16. But for multiples of
> three print "Fizz" instead of the number and for the multiples of five print
> "Buzz". For numbers which are multiples of both three and five print
> "FizzBuzz".

#### What's the usual approach?

Lets start with a procedural, object-oriented language, Javascript. The problem
starts with "print the numbers from 1 to 30". So lets pull out our venerable
for-loop:

```klipse-es2017
for (i = 1; i <= 16; i++) {
  console.log(i);
}
```

Next, just adjust it to do what we want.

```klipse-es2017
for (i = 1; i <= 16; i++) {
  if ( i % 15 === 0 ) {
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

- Click to see the docs --> [`range`](http://cljs.github.io/api/cljs.core/range){:target="_blank"}
- Can we make it finite? [`take`](http://cljs.github.io/api/cljs.core/take){:target="_blank"}
- Can we chop off the first element? [`rest`](http://cljs.github.io/api/cljs.core/rest){:target="_blank"}
- Can we make a sequence of results of a function applied to each
  element? [`map`](http://cljs.github.io/api/cljs.core/map){:target="_blank"}

```klipse
(range)
```

What about the Fizz's and Buzz's? How about a list where every third item is
"Fizz", and we just make the others empty. The ClojureScript function [`cycle`](http://cljs.github.io/api/cljs.core/cycle){:target="_blank"}
Makes a new _infinite_ list by repeating the given one over and over: 

- Can we combine the elements of the Fizz list with those of the Buzz list
  giving a sequence of [`vector`](http://cljs.github.io/api/cljs.core/vector){:target="_blank"}s? [`map`](http://cljs.github.io/api/cljs.core/map){:target="_blank"}
- Then also combine the list of numbers.

```klipse
(take 16 (cycle ["" "" "Fizz"]))
```

Now we have the sequence of numbers and strings we want.

- Can we make the annonymous function more succinct? [`#`](http://cljs.github.io/api/syntax/function){:target="_blank"}
- Can we print each element, one to a line? [`prn`](http://cljs.github.io/api/cljs.core/prn){:target="_blank"}

```klipse
(take 16 (map (fn [fz bz n] (-> (str fz bz) not-empty (or n)))
              (cycle ["" "" "Fizz"])
              (cycle ["" "" "" "" "Buzz"])
              (rest (range))))
```


{% include klipse.html %}

