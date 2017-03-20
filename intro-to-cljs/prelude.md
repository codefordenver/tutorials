
{% include pre-defs.html %}

# Intro to Clojure(script)
Michelle Lim + Tyler Perkins // Code for Denver // 3-20-17

![hi](https://media.giphy.com/media/L3nWlmgyqCeU8/giphy.gif)

# It all starts with an *expression*
# *`(function  x  y  z)`* 
## _\*do **function** to **x**, **y**, and **z**\*_

---

### Example 1:
## `(+ 1 2 3)`
_\*adds 1, 2, and 3\*_
## `=> 6`
*returns six*

---

### Example 2:
## `(= 1 2)`
_\*the equality of 1 and 2 is...\*_
## `=> false`
_returns false_

---

### Example 3:
## `(if (= 1 1) "yes" "no")`
_\*if `(= 1 1)` is true, return "yes", otherwise return "no"\*_
## `=> "yes"`
_returns the third item_

---

# Expressions within expressions

### Example 4:
## `(= (+ 1 2 3) 5)`
Complete the expression below
```klipse
(+ 1 2 3)
```

### Example 5:
## `(if (= (+ 1 2 3) 5) "yes" "no")`
Complete the expression below
```klipse
(= (+ 1 2 3) 5)
```

---

# Parentheses = (hugs) for your code. Perfect for nesting.
![](https://canvas-files-prod.s3.amazonaws.com/uploads/af080d5d-b6c1-4f21-9c8e-1962479b5514/undefined)
# (❤)


# Clojure is a general purpose language, designed to be hosted on top of another language
![](https://canvas-files-prod.s3.amazonaws.com/uploads/bde1c59a-a9f0-41d0-91f5-7ce4292f9313/undefined)
  - **Clojure**  runs on the **Java Virtual Machine (JVM)**
  
![](https://s3.amazonaws.com/stufff/cljs.png)
  - **ClojureScript**  compiles to **Javascript**

---

# Tools (clickable):
[![](https://s3.amazonaws.com/stufff/re-frame.png)](https://github.com/Day8/re-frame){:target="_blank"}
[![](https://s3.amazonaws.com/stufff/lein.jpg)](https://leiningen.org/){:target="_blank"}
[![](https://s3.amazonaws.com/stufff/figwheel.png)](https://github.com/bhauman/lein-figwheel){:target="_blank"}

## [live demo: [codefordenver/owlet-ui](https://github.com/codefordenver/owlet-ui){:target="_blank"}]

---

# Clojure is dialect of Lisp: the second oldest high-level programming language widely used today

![](https://canvas-files-prod.s3.amazonaws.com/uploads/56f9a117-421b-4d8c-82f1-815be3aee2d6/lisp-cycles-edit.png) [[source]](https://xkcd.com/297/){:target="_blank"}

## Lisp timeline:
![](https://upload.wikimedia.org/wikipedia/en/timeline/71b23fdea3320ea89f55ced33e678ab2.png)
 
---

# A different paradigm

Lisps are _**functional**_ programming languages.

Javascript, Python, Ruby, C#, and C++ are all _**object-oriented**_ programming languages.

### Context: Data can be passed around by *value* or *reference*
- A data **value** is stored in a specific location in memory
- Most languages pass a **reference** to that location instead of the value itself, manipulating data *in place*
- In the early days memory was limited, so the focus was on reusing space

## Object-oriented approach

**A system based on objects**

![](https://docs.oracle.com/javase/tutorial/figures/java/concepts-bicycleObject.gif)
[[source]](https://docs.oracle.com/javase/tutorial/java/concepts/object.html){:target="_blank"}

**and specific rules for how they can communicate.**

![](https://s3.amazonaws.com/stufff/oop-street.jpg)

- Passes data by **reference**. Actual object values are hidden from the rest (encapsulated)
- Objects interact by sending messages and calling methods
- Methods often produce *side effects*

>"In order to safely work with the data you have to know all the places where it might be referenced. The complexity grows with the size of the application. The more code has access to a piece of data the more proverbial balls you end up having to juggle in your head."
—*[Clojure Distilled](https://yogthos.github.io/ClojureDistilled.html){:target="_blank"}*

   
# Functional approach

## Defines standalone actions (expressions) that are reuseable and composeable
- Passes data by ***value***. Every expression returns a new, transformed copy of the data
- Composes expressions within a larger expression, which becomes the system itself
- The language takes care of figuring out which parts can be cleaned up when they're no longer used

![](https://s3.amazonaws.com/stufff/fp-fish.jpg)
[[source]](http://www.huffingtonpost.com/john-pavley/teach-a-kid-functional-pr_b_3666853.html)


---
 

# Functions we'll use in Programming Functionally, next

- str - `(str 2 "legit" 2 "quit")`
- rest - `(rest [0 1 2 3])`
- cycle - `(cycle ["one" "two" "three"])`
- take - `(take 6 (cycle ["one" "two" "three"]))`
- [map](https://youtu.be/e-5obm1G_FY?t=11m23s){:target="_blank"}
  - a ***higher-order function***; can take functions as inputs, and return functions as output
  - `(map inc [0 1 2 3])`
  - `(map + [0 1 2 3] [4 5 6 7])`

First, let's take them for a spin. Evaluate the examples above using the editor below.

```klipse
; Use my-doc to see the docs for a function:
(my-doc #'str)
```

[cljs cheatsheet](http://cljs.info/cheatsheet/){:target="_blank"}

---

![](https://s3.amazonaws.com/stufff/Boacious.gif)

# A note on laziness

`map`, `take`, and `cycle` return "lazy" sequences;

with a lazy sequence, the function returns both an item ***and the next function in the sequence***,

providing a ***natural*** way to produce infinite data structures..!

---

[Next: Programming Functionally →](programming-functionally.html)


{% include klipse.html %}
