
## Introduction to ClojureScript

There are _soooo_ many programming languages in wide use today, even just for
web development. So what makes ClojureScript special, and how can it enhance
and enlighten your life as a developer?

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

- Can we make it finite? `take`
- Can we chop off the first element? `rest`
- Can we make a sequence of results of a function applied to each
  element? `map`

```klipse
(range)
```

What about the Fizz's and Buzz's? How about a list where every third item is
"Fizz", and we just make the others empty. The ClojureScript function `cycle`
Makes a new _infinite_ list by repeating the given one over and over: 

- Can we combine the elements of the Fizz list with those of the Buzz list
  giving a sequence of `vector`s? `map`
- Then also combine the list of numbers.

```klipse
(take 16 (cycle ["" "" "Fizz"]))
```

Now we have the sequence of numbers and strings we want.

- Can we make the annonymous function more succinct? `#`
- Can we print each element, one to a line? 'prn'

```klipse
(take 16 (map (fn [fz bz n] (-> (str fz bz) not-empty (or n)))
              (cycle ["" "" "Fizz"])
              (cycle ["" "" "" "" "Buzz"])
              (rest (range))))
```


{% include klipse.html %}

