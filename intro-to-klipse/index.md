
## Introduction to Klipse

This page demonstrates the use of Klipse in a tutorial. Explanitory material
is easy to write, owing to the simple Markdown format, yet the result can be
clearly presented on a big screen by a teacher. Intuitively scrolling through
the exposition, she can elucidate difficult points by live coding within any of
the Klipse code snippets. Later, each of her students can review the very same
page, interactively experimenting with each new detail as their understanding
grows.

### Klipse Demo

We can easily present ClojureScript code in a nice document like this, then
modify it and demonstrate what it does.

```klipse
(def salutation "Hello, world!")
salutation
```

\\
\\
\\
<!-- Above is a simple way to add blank lines, but must be ended
     by a line which is not \\. Otherwise a single "\" appears.
     Alternatively, just use <br/>:
-->
<br/>
<br/>

Global definitions like `salutation` above are visible through the whole page:

<pre id="toggle-me"><code class="language-klipse">

(require '[clojure.string :as str])
(let [hi (str/upper-case salutation)]
  ;(js/window.alert hi)
  (println hi))

</code></pre>

Notice how we see both the printed value and the result of the expression
(`nil`). We can also require other namespaces, as shown.

<a onclick="document.getElementById('toggle-me').hidden = ! document.getElementById('toggle-me').hidden;">
And we can toggle the display of the above Klipse code snippet!
</a>

\\
\\
\\
\\
\\
For comparison, we can also demonstrate JavaScript code:

```klipse-es2017
const maybeAlert = b => {
  if ( b ) {
    window.alert("42")
    return "yup"
  } else {
    return "nope"
  }
}

maybeAlert(false)
```

<!-- Or use ```eval-js above for es2015

Note that the following include directive must appear at the BOTTOM of the file.
-->

{% include klipse.html %}

