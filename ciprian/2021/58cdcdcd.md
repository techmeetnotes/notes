## -- title:       Regarding a common HTML rendering for markup languages
## -- library:     ideas
## -- identifier:  58cdcdcd
## -- format:      commonmark
## -- timestamp:   2021-09-13 16:36:56


## The problem

At the moment there are many (wiki and generic) markup languages -- e.g. CommonMark, reStructuredText, MoinMoin, [Mycomarkup](<https://mycorrhiza.wiki/help/en/mycomarkup>), etc. -- all of which have as primary target HTML rendering.  Unfortunately, although all of these markups are almost 100% semantically compliant in what they can describe, namely the all work with the same building-blocks (headings, paragraphs, lists, code, blockquotes, etc.), the resulting HTML is most of the time different.

Granted, each of these do use `<p>` for paragraphs, `<code>` for code, etc., but for more complex there is no common output.

For example CommonMark compliant rendering engines (including `cmark`, and `goldmark` for Go), use the following rendering of code blocks:
~~~~
````
some code
```` -> ````
<pre><code>
some code
</code></pre>
````
~~~~

On the other hand, reStructuredText via its `rst2html` yields the following:
~~~~
````
::

    some code
```` -> ````
<pre class="literal-block">
some code
</pre>
````
~~~~

MoinMoin yields the following:
~~~~
````
{{{
some code
}}}
```` -> ````
<pre>
<span id="line-1-1" class="anchor"></span>
some code
</pre>
````
~~~~




## Why should one care?

In the end the resulting HTML is meant to be interpreted and displayed by the browser, and as long as the output is "visually as expected", the rest shouldn't count for much.

However, imagine one is writing a wiki engine that wants to support multiple markup languages, one would have to use some CSS to make the pages look "pretty".  Unfortunately, due to the fact that many of these generate different looking HTML's, it is hard to create a CSS that applies uniformly to all outputs.

Besides that, having a common HTML structure, one could write tools that interact with the resulting HTML documents, for example to summarize, extract information, or prepare for presentation in other formats (i.e. printing).  This would imply using a DOM parser, but the code needed to "pattern match" the same semantic construct in each output HTML would be quite tedious.




## A possible solution

The only way out might be to standardize on a common HTML rendering structure, including attributes, that could be implemented (perhaps as alternatives) in the various markup language renderers.

Such a specification would state things such as:
* how code blocks are rendered?  e.g. `<pre><code>...</code></pre>` or just `<pre>...</pre>`?
* how are code blocks languages annotated?  e.g. `goldmark` uses `class="language-whatever"`;
* what are the allowed "block" nodes?  e.g. `<p>`, `<blockquote>`, `<ul>`, `<li>`, etc.;
* what are the allowed "block" nestings?  e.g. `<blockquote>` can contain `<p>`, `<ul>`, but not `<h1>`, etc.;
* a set of common classes to be used for what reStructuredText and MoinMoin call "admonitions";
* (for those that care) how should emoticons be marked?  e.g. `<span class="emoticon">:)</span>`, etc.;




## Universal markup

At some point in my discussions with Lion Kimbro, he mentioned the concept of "universal markup" (?), that can be used as an "exchange markup language" between different markup languages.  I.e. one could take a document from CommonMark, translate it into "universal markup", translate it into reStructuredText, then again into "universal markup", and now back to CommonMark, without losing neither semantic nor formatting;  the syntax might be different, but not the rendering.

Granted, such a "universal markup" would be possible only for the common denominator of all markup languages, but given that most of them have a large overlap, it should be possible to implement.

How would such a "universal markup" benefit from the common HTML rendering?  Given that one knows what are the rules of the rendered HTML, one can reverse-engineer, sort of a disassemble, back to an AST (abstract syntax tree), and from there into another markup.




## What follows next?

It would be nice that markup language creators and (maybe even better) markup parser developers come together and try to get to a common ground about how the rendered HTML should look like.

Is there such interest?  If so, let's discuss on Lion Kimbro's Discourse [#wiki](<https://discord.com/channels/695746339019030619/824342692505845840>) channel.
