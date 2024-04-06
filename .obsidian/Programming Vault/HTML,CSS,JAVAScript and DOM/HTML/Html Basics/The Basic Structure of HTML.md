
1. A element has a opening tag, the content and a closing tag.
2. Opening tag usually has attributes.
3. Attributes are responsible for changing the behavior of the element. Attributes contain extra information about the element that you don't want to appear in the actual content.
4. The element makes that marked up content look a certain way
5. Attribute syntax: `<element nameOfAttribute="attributeValue">`.  It is legal to not quote values that don't have whitespace but it is bad practice and should be avoided.
6. We can nest one element inside another element.
7.  Void element:  **cannot** have any child nodes and does not wrap content up, therefore it has one tag which is where the attributes are implemented.
8. Anything in HTML between `<!--` and `-->` is an **HTML comment**.
9. For accessibility reasons you should use the elements for what they where created to do and not for your own goals meaning that you should not use a heading element to draw attention to the content of it. Even do it is legal to do, heading element's have the purpose of dividing the body in chapter's and to organize the webpage. Blind people will not know what you meant and they will be confused as fuck.


This is a setup that you will se a lot, let's understand what is going on:

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>My test page</title>
  </head>
  <body>
    <img src="images/firefox-icon.png" alt="My test image" />
  </body>
</html>

```

* `<!doctype html>`: preamble to tell the browser to use no-quirks/full standards  mode.
* `<html></html>`: Element that wraps the entire page up, the root element. `lang` attribute for the language of the program. This is useful in many ways. Your HTML document will be indexed more effectively by search engines if its language is set. You can also set subsections of your document to be recognized as different languages: `<p>Japanese example: <span lang="ja">ご飯が熱い。</span>.</p>
`
* `<head></head>` — the [`<head>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head) element. This element acts as a container for all the stuff you want to include on the HTML page that _isn't_ the content you are showing to your page's viewers. Contains the metadata: data that describes itself, in this case the html document.
* `<meta charset="utf-8">` — This element sets the character set your document should use to UTF-8 which includes most characters from the vast majority of written languages. Essentially, it can now handle any textual content you might put on it. There is no reason not to set this.
* - `<meta name="viewport" content="width=device-width">` — This [viewport element](https://developer.mozilla.org/en-US/docs/Web/CSS/Viewport_concepts#mobile_viewports) ensures the page renders at the width of viewport, preventing mobile browsers from rendering pages wider than the viewport and then shrinking them down.
* - `<title></title>` — the [`<title>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/title) element. This sets the title of your page, which is the title that appears in the browser tab the page is loaded in. It is also used to describe the page when you bookmark/favorite it.
* `<body></body>` — the [`<body>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/body) element. This contains _all_ the content that you want to show to web users when they visit your page.
* `<img />`: a void element that put's an image that is in the `src` attribute path in your html page. `alt` attribute allow for it's value to be displayed on the page when the image cannot be loaded, it should desridbe to the reader what is the image about.

### Headings

. HTML contains 6 heading levels, `[<h1> - <h6>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements), although you'll commonly only use 3 to 4 at most. Super important for organizing the text in your webpage.

`h2` elements are sub-headings of `h1` elements. `h3` elements are sub-headings of `h2` elements and so on. 

You should aim to not use more than 3 heading levels per page so the page does not become hard to navigate.

Search engines indexing your page consider the contents of headings as important keywords for influencing the page's search rankings.

Users looking at a web page tend to scan quickly to find relevant content, often just reading the headings, to begin with.

You can use heading level elements to style a whole section of the web page with CSS or determine the whole logic of a heading level or a specific heading level element (a heading level element that has a value for it's `id` attribute). 
### What Can You Find Inside The Head?

* `title` element, already discussed. Title and H1 are 2 completely different things.
* Every `link` void element goes inside the head.
- The [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) element should always go inside the head of your document. This takes two attributes, `rel="stylesheet"`, which indicates that it is the document's stylesheet, and `href`, which contains the path to the stylesheet file:
    
    ```
    <link rel="stylesheet" href="my-css-file.css" />
    ```
    
- The [`<script>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) element should also go into the head, and should include a `src` attribute containing the path to the JavaScript you want to load, and `defer`, which basically instructs the browser to load the JavaScript after the page has finished parsing the HTML. This is useful as it makes sure that the HTML is all loaded before the JavaScript runs, so that you don't get errors resulting from JavaScript trying to access an HTML element that doesn't exist on the page yet. There are actually a number of ways to handle loading JavaScript on your page, but this is the most reliable one to use for modern browsers (for others, read [Script loading strategies](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript#script_loading_strategies)).
    
    ```
    <script src="my-js-file.js" defer></script>
    ```
    
    **Note:** The `<script>` element may look like a [void element](https://developer.mozilla.org/en-US/docs/Glossary/Void_element), but it's not, and so needs a closing tag. Instead of pointing to an external script file, you can also choose to put your script inside the `<script>` element.
    
* `<link rel="icon" href="favicon.ico" type="image/x-icon" />`: creates and icon for your html page. These will be displayed in certain contexts. The most commonly used of these is the **favicon** (short for "favorites icon", referring to its use in the "favorites" or "bookmarks" lists in browsers).  Another way to add an icon to you page is by saving it in the same directory as the site's index page, saved in `.ico` format (most also support favicons in more common formats like `.gif` or `.png`). Attributes: `rel` determines the *relationship* between `href` value and the document. `type` determines what is the type of the element, in this case what is the type of `link` that we are creating.

* `<meta>`: even do this is short for metadata everything inside the head is metadata. You will  use this element to, through different attributes, implement all types of metadata in you  program. For example:  `<meta charset="utf-8" />' determines the character set metadata for the html document. Utf-8 supports any language so it should be the option almost everytime.
* You can use `name` and `content` attributes of the `<meta>` element to determine a lot of different type of metadata:
```html
<meta name="author" content="Chris Mills" />
<meta
  name="description"
  content="The MDN Web Docs Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing websites and applications." />

Implementing author and description metadata.

```

Now search for "MDN Web Docs" in your favorite search engine. You'll notice the description `<meta>` and `<title>` element content used in the search result — definitely worth having!

Many `<meta>` features just aren't used anymore. For example, the keyword `<meta>` element (`<meta name="keywords" content="fill, in, your, keywords, here">`) — which is supposed to provide keywords for search engines to determine the relevance of that page for different search terms — is ignored by search engines, because spammers were just filling the keyword list with hundreds of keywords, biasing results.

#### Proprietary Metadata

 Many of the features you'll see on websites are proprietary creations designed to provide certain sites (such as social networking sites) with specific information they can use.

In the MDN Web Docs sourcecode, you'll find this:

```html
<meta
  property="og:image"
  content="https://developer.mozilla.org/mdn-social-share.png" />
<meta
  property="og:description"
  content="The Mozilla Developer Network (MDN) provides
information about Open Web technologies including HTML, CSS, and APIs for both websites
and HTML Apps." />
<meta property="og:title" content="Mozilla Developer Network" />

```

One effect of this is that when you link to MDN Web Docs on Facebook, the link appears along with an image and description: a richer experience for users.

### Void Elements
#### What are the Void Elements?

- [`<area>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/area)
- [`<base>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base)
- [`<br>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/br)
- [`<col>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/col)
- [`<embed>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/embed)
- [`<hr>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/hr)
- [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img)
- [`<input>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)
- [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link)
- [`<meta>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)
- [`<param>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/param) Deprecated
- [`<source>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/source)
- [`<track>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/track)
- [`<wbr>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/wbr)

#### Why Void Elements End With /?

 HTML parsers ignore that slash character. 
 
 This is important to remember when an element such as [`<script>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) or [`<ul>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul) does require a closing tag. In this case, adding a trailing slash in the start tag does not close the element.

You should add trailing slash character to the start tags of void elements to make them XHTML-compatible and more readable. For example, some code formatters will convert `<input type="text">` to `<input type="text" />`.

Self-closing tags are required in void elements in [XML](https://developer.mozilla.org/en-US/docs/Glossary/XML), [XHTML](https://developer.mozilla.org/en-US/docs/Glossary/XHTML), and [SVG](https://developer.mozilla.org/en-US/docs/Glossary/SVG) (e.g., `<circle cx="50" cy="50" r="50" />`).

In SVG and MathML, elements that cannot have any child nodes are allowed to be marked as self-closing. In such cases, if an element's start tag is marked as self-closing, the element must not have an end tag.


### Block and Inline Elements(CSS)

A **block** on a webpage is an [HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML) [element](https://developer.mozilla.org/en-US/docs/Glossary/Element) that appears on a new line, i.e. underneath the preceding element in a horizontal writing mode, and above the following element (commonly known as a _block-level element_). For example, [`<p>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/p) is by default a block-level element, whereas [`<a>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) is an _inline element_

Using the [`display`](https://developer.mozilla.org/en-US/docs/Web/CSS/display) property you can change whether an element displays inline or as a block (among many other options); **blocks** are also subject to the effects of positioning schemes and use of the [`position`](https://developer.mozilla.org/en-US/docs/Web/CSS/position) property.
### Links

#### How To Navigate relative URL path's
Links need URLs .URLs use paths to find files. Paths specify where the file you're interested in is located in the filesystem. Links should be created using relative paths so given the current `.html` file as the context. 
*  For URL path in the same directory you just need to type the name of the target file|directory.
* For URL path in a subdirectory:` subDirectoryName/targetFile|DirectoryNam`e.
* For URL path in the parent directory:` ../targetFile|DirectoryName`.

`<a href="http://www.hypertextreference.com/someWebPage">content</a>`: short for "anchor".  
Used to create internal or external hyperlinks. 
When you click on the content it takes you to the hypertext reference determined by the `href` attribute, this is clearly a link by the way. You might get unexpected results if you omit the `https://` or `http://` part, called the _protocol_.
Attributes:
* title: displays it's value when you hover over the hyperlink on the web page.
* download: it's value determines the default name of the file on the client's computer that is about to be downloaded.
* Not an attribute but to link to an email you can use: `<a href="mailto:nowhere@mozilla.org">Send email to nowhere</a>`. In fact, the email address is optional. If you omit it and your [`href`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#href) is "mailto:", a new outgoing email window will be opened by the user's email client with no destination address. This is often useful as "Share" links that users can click to send an email to an address of their choosing.

`<link>` explained in What Can You Find Inside The Head?.

`<img>` (image) link: to do that just wrap the `img`  element in a `<a>`. When you click the image you will activate the link and go to wherever you want to go.

Almost any content can be made into a link, even [block-level elements](https://developer.mozilla.org/en-US/docs/Glossary/Block/CSS).

Don't repeat the URL as part of the link text — URLs look ugly, and sound even uglier when a screen reader reads them out letter by letter.
Don't say "link" or "links to" in the link text — it's just noise. 
Keep your link text as short as possible — this is helpful because screen readers need to interpret the entire link text.
Minimize instances where multiple copies of the same text are linked to different places. 
When linking to a resource that will be downloaded (like a PDF or Word document), streamed (like video or audio), or has another potentially unexpected effect (opens a popup window), you should add clear wording to the link in order to reduce any confusion.

If you REALLY want to know everything about email links read this:
#### Specifying details of Email Links

In addition to the email address, you can provide other information. In fact, any standard mail header fields can be added to the `mailto` URL you provide. The most commonly used of these are "subject", "cc", and "body" (which is not a true header field, but allows you to specify a short content message for the new email). Each field and its value is specified as a query term.

Here's an example that includes a cc, bcc, subject and body:

```html
<a
  href="mailto:nowhere@mozilla.org?cc=name2@rapidtables.com&bcc=name3@rapidtables.com&subject=The%20subject%20of%20the%20email&body=The%20body%20of%20the%20email">
  Send mail with cc, bcc, subject and body
</a>

```


#### Document fragments

It's possible to link to a specific part of an HTML document, known as a **document fragment**, rather than just to the top of the document. To do this you first have to assign an [`id`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes#id) attribute to the element you want to link to. It normally makes sense to link to a specific heading, so this would look something like the following:

```
<h2 id="Mailing_address">Mailing address</h2>
```

Then to link to that specific `id`, you'd include it at the end of the URL, preceded by a hash/pound symbol (`#`), for example:

```
<p>
  Want to write us a letter? Use our
  <a href="contacts.html#Mailing_address">mailing address</a>.
</p>
```

You can even use the document fragment reference on its own to link to _another part of the current document_:

```
<p>
  The <a href="#Mailing_address">company mailing address</a> can be found at the
  bottom of this page.
</p>
```



### Text Formatting

#### [Paragraphs](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics#paragraphs)

`<p></p>`

#### Lists

unordered list: `<ul></ul>`
ordered list: `<ol></ol>`
to nest list items inside the lists:
list items : `<li>content</li>`

Descriptive list wrapper: `<dl></dl>`
Descriptive list item title: `<dt></dt>`
Descriptive list item description: `<dd></dd>`
One title can have multiple descriptions 


You can nest one list inside another list.
#### Emphasis, Strong Importance, Italic, Bold and Underlined

`<em>`: should be used when we want to add emphasis to a word. Which can change the meaning of the sentence: 
I am glad you weren't late. <!-- This is genuine-->
I am _glad_ you weren't _late_. <!-- This is ironic or passive aggressive-->

This element will change apply italic to it's content by default but that is not the purpose of the element . If you want the content just to be italic use `<i>` or wrap the content in a `<span>` element and style the span element in a CSS file.

 `<strong>`(strong importance): should be used to wrap text that is important, text that you want your viewer to see and take very seriously. 
 
This element will apply bold to it's content by default but that is not the purpose of the element . If you want the content just to be bold use `<b>` or wrap the content in a `<span>` element and style the span element in a CSS file.

- `<i>` (italic) is used to convey a meaning traditionally conveyed by italic: foreign words, taxonomic designation, technical terms, a thought…
- [`<b>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/b)  (bold) is used to convey a meaning traditionally conveyed by bold: keywords, product names, lead sentence…
- [`<u>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/u)  (underlined) is used to convey a meaning traditionally conveyed by underline: proper name, misspelling…

`<em>` and `<strong>` should have priority over `<i>`, `<b>` and `<u>`.

- Use [`<em>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/em) to indicate stress emphasis.
- Use [`<strong>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/strong) to indicate importance, seriousness, or urgency.
- Use [`<mark>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/mark) to indicate relevance.
- Use [`<cite>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/cite) to mark up the name of a work, such as a book, play, or song.
- Use [`<dfn>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dfn) to mark up the defining instance of a term.

#### Quotations

`Blockquote`: used to wrap up a paragraph or multiple paragraphs that quote something. Blockquote elements are indented by default in the webpage.
attributes:
* cite: value should be the URL containing the quote's source (where you took that quote from).

```html
<p>Here is a blockquote:</p>
<blockquote
  cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote">
  <p>
    The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or
    <em>HTML Block Quotation Element</em>) indicates that the enclosed text is
    an extended quotation.
  </p>
</blockquote>
```

#### Inline Quotations

`<q>` (quotations): does the same thing as blockquotes but for inline quotations, quotations that are smaller than a paragraph element.
attributes:
* Idk if this is true but I think the attributes are the same of Blockquote elements

#### Citations

`<cite>` : makes the quotation's source available on the page. It's content should be the title of the resource being quoted. Nothing stops you from wrapping up `<cite>` in a `<a>` that takes you to the quote's source when you click on it:
```html
<p>
  According to the
  <a href="/en-US/docs/Web/HTML/Element/blockquote">
    <cite>MDN blockquote page</cite></a>:
</p>

<blockquote
  cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote">
  <p>
    The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or
    <em>HTML Block Quotation Element</em>) indicates that the enclosed text is
    an extended quotation.
  </p>
</blockquote>

<p>
  The quote element — <code>&lt;q&gt;</code> — is
  <q cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q">
    intended for short quotations that don't require paragraph breaks.
  </q>
  — <a href="/en-US/docs/Web/HTML/Element/q"><cite>MDN q page</cite></a>.
</p>
```

#### Contact Details

HTML has an element for marking up contact details — [`<address>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/address). This wraps around your contact details, for example:

```html
<address>Chris Mills, Manchester, The Grim North, UK</address>
```

The [`<address>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/address) element should only be used to provide contact information for the document contained with the nearest [`<article>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/article) or [`<body>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/body) element. It would be correct to use it in the footer of a site to include the contact information of the entire site, or inside an article for the contact details of the author, but not to mark up a list of addresses unrelated to the content of that page.

It could also include more complex markup, and other forms of contact information, for example:

```html
<address>
  <p>
    Chris Mills<br />
    Manchester<br />
    The Grim North<br />
    UK
  </p>

  <ul>
    <li>Tel: 01234 567 890</li>
    <li>Email: me@grim-north.co.uk</li>
  </ul>
</address>
```


You can also link to a page that has the contact info:
`<address> Page written by <a href="../authors/chris-mills/">Chris Mills</a> </address>`

#### SuperScript SubScript

`<sup>`: superscripts(to put something on top) a number or letter.
`<sub>`: subscripts(to put something on the bottom) a number or letter

#### Represent Computer Code

Very good way to remember the code related html elements:
In HTML the CODE can have it's text PREformatted and also mark's VARiables, input from the KeyBoarD and the output SAMples up.

- [`<code>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/code): For marking up generic pieces of computer code.
- [`<pre>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/pre): For retaining whitespace (generally code blocks) — if you use indentation or excess whitespace inside your text, browsers will ignore it and you will not see it on your rendered page. If you wrap the text in `<pre></pre>` tags however, your w itespace will be rendered identically to how you see it in your text editor.
- [`<var>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/var): For specifically marking up variable names.
- [`<kbd>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/kbd): For marking up keyboard (and other types of) input entered into the computer.
- [`<samp>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/samp): For marking up the output of a computer program.


```html
<pre><code>const para = document.querySelector('p');

para.onclick = function() {
  alert('Owww, stop poking me!');
}</code></pre>

<p>
  You shouldn't use presentational elements like <code>&lt;font&gt;</code> and
  <code>&lt;center&gt;</code>.
</p>

<p>
  In the above JavaScript example, <var>para</var> represents a paragraph
  element.
</p>

<p>Select all the text with <kbd>Ctrl</kbd>/<kbd>Cmd</kbd> + <kbd>A</kbd>.</p>

<pre>$ <kbd>ping mozilla.org</kbd>
<samp>PING mozilla.org (63.245.215.20): 56 data bytes
64 bytes from 63.245.215.20: icmp_seq=0 ttl=40 time=158.233 ms</samp></pre>
```

#### Time and Date

```html
<!-- Standard simple date -->
<time datetime="2016-01-20">20 January 2016</time>
<!-- Just year and month -->
<time datetime="2016-01">January 2016</time>
<!-- Just month and day -->
<time datetime="01-20">20 January</time>
<!-- Just time, hours and minutes -->
<time datetime="19:30">19:30</time>
<!-- You can do seconds and milliseconds too! -->
<time datetime="19:30:01.856">19:30:01.856</time>
<!-- Date and time -->
<time datetime="2016-01-20T19:30">7.30pm, 20 January 2016</time>
<!-- Date and time with timezone offset -->
<time datetime="2016-01-20T19:30+01:00">
  7.30pm, 20 January 2016 is 8.30pm in France
</time>
<!-- Calling out a specific week number -->
<time datetime="2016-W04">The fourth week of 2016</time>
```

#### Abbreviations
`<abbr>`: used to wrap abbreviations up.
### Web Page Structure

[header:](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure#header)

Usually a big strip across the top with a big heading, logo, and perhaps a tagline. This usually stays the same from one webpage to another.

[navigation bar:](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure#navigation_bar)

Links to the site's main sections; usually represented by menu buttons, links, or tabs. Like the header, this content usually remains consistent from one webpage to another — having inconsistent navigation on your website will just lead to confused, frustrated users. Many web designers consider the navigation bar to be part of the header rather than an individual component, but that's not a requirement; in fact, some also argue that having the two separate is better for [accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility), as screen readers can read the two features better if they are separate.

[main content:](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure#main_content)

A big area in the center that contains most of the unique content of a given webpage, for example, the video you want to watch, or the main story you're reading, or the map you want to view, or the news headlines, etc. This is the one part of the website that definitely will vary from page to page!

[sidebar:](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure#sidebar)

Some peripheral info, links, quotes, ads, etc. Usually, this is contextual to what is contained in the main content (for example on a news article page, the sidebar might contain the author's bio, or links to related articles) but there are also cases where you'll find some recurring elements like a secondary navigation system.

[footer:](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure#footer)

A strip across the bottom of the page that generally contains fine print, copyright notices, or contact info. It's a place to put common information (like the header) but usually, that information is not critical or secondary to the website itself. The footer is also sometimes used for [SEO](https://developer.mozilla.org/en-US/docs/Glossary/SEO) purposes, by providing links for quick access to popular content.

html elements:
- **header:** [`<header>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/header).
- **navigation bar:** [`<nav>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/nav).
- **main content:** [`<main>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/main), with various content subsections represented by [`<article>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/article), [`<section>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/section), and [`<div>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/div) elements.
- **sidebar:** [`<aside>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/aside); often placed inside [`<main>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/main).
- **footer:** [`<footer>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/footer).

- [`<main>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/main) is for content _unique to this page._ Use `<main>` only _once_ per page, and put it directly inside [`<body>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/body). Ideally this shouldn't be nested within other elements.
- [`<article>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/article) encloses a block of related content that makes sense on its own without the rest of the page (e.g., a single blog post).
- [`<section>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/section) is similar to `<article>`, but it is more for grouping together a single part of the page that constitutes one single piece of functionality (e.g., a mini map, or a set of article headlines and summaries), or a theme. It's considered best practice to begin each section with a [heading](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/HTML_text_fundamentals); also note that you can break `<article>`s up into different `<section>`s, or `<section>`s up into different `<article>`s, depending on the context.
- [`<aside>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/aside) contains content that is not directly related to the main content but can provide additional information indirectly related to it (glossary entries, author biography, related links, etc.).
- [`<header>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/header) represents a group of introductory content. If it is a child of [`<body>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/body) it defines the global header of a webpage, but if it's a child of an [`<article>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/article) or [`<section>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/section) it defines a specific header for that section (try not to confuse this with [titles and headings](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML#adding_a_title)).
- [`<nav>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/nav) contains the main navigation functionality for the page. Secondary links, etc., would not go in the navigation.
- [`<footer>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/footer) represents a group of end content for a page.

#### Big Ass Example Of The Use OF theses Elements
```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />

    <title>My page title</title>
    <link
      href="https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300|Sonsie+One"
      rel="stylesheet" />
    <link rel="stylesheet" href="style.css" />
  </head>

  <body>
    <!-- Here is our main header that is used across all the pages of our website -->

    <header>
      <h1>Header</h1>
    </header>

    <nav>
      <ul>
        <li><a href="#">Home</a></li>
        <li><a href="#">Our team</a></li>
        <li><a href="#">Projects</a></li>
        <li><a href="#">Contact</a></li>
      </ul>

      <!-- A Search form is another common non-linear way to navigate through a website. -->

      <form>
        <input type="search" name="q" placeholder="Search query" />
        <input type="submit" value="Go!" />
      </form>
    </nav>

    <!-- Here is our page's main content -->
    <main>
      <!-- It contains an article -->
      <article>
        <h2>Article heading</h2>

        <p>
          Lorem ipsum dolor sit amet, consectetur adipisicing elit. Donec a diam
          lectus. Set sit amet ipsum mauris. Maecenas congue ligula as quam
          viverra nec consectetur ant hendrerit. Donec et mollis dolor. Praesent
          et diam eget libero egestas mattis sit amet vitae augue. Nam tincidunt
          congue enim, ut porta lorem lacinia consectetur.
        </p>

        <h3>Subsection</h3>

        <p>
          Donec ut librero sed accu vehicula ultricies a non tortor. Lorem ipsum
          dolor sit amet, consectetur adipisicing elit. Aenean ut gravida lorem.
          Ut turpis felis, pulvinar a semper sed, adipiscing id dolor.
        </p>

        <p>
          Pelientesque auctor nisi id magna consequat sagittis. Curabitur
          dapibus, enim sit amet elit pharetra tincidunt feugiat nist imperdiet.
          Ut convallis libero in urna ultrices accumsan. Donec sed odio eros.
        </p>

        <h3>Another subsection</h3>

        <p>
          Donec viverra mi quis quam pulvinar at malesuada arcu rhoncus. Cum
          soclis natoque penatibus et manis dis parturient montes, nascetur
          ridiculus mus. In rutrum accumsan ultricies. Mauris vitae nisi at sem
          facilisis semper ac in est.
        </p>

        <p>
          Vivamus fermentum semper porta. Nunc diam velit, adipscing ut
          tristique vitae sagittis vel odio. Maecenas convallis ullamcorper
          ultricied. Curabitur ornare, ligula semper consectetur sagittis, nisi
          diam iaculis velit, is fringille sem nunc vet mi.
        </p>
      </article>

      <!-- the aside content can also be nested within the main content -->
      <aside>
        <h2>Related</h2>

        <ul>
          <li><a href="#">Oh I do like to be beside the seaside</a></li>
          <li><a href="#">Oh I do like to be beside the sea</a></li>
          <li><a href="#">Although in the North of England</a></li>
          <li><a href="#">It never stops raining</a></li>
          <li><a href="#">Oh well…</a></li>
        </ul>
      </aside>
    </main>

    <!-- And here is our main footer that is used across all the pages of our website -->

    <footer>
      <p>©Copyright 2050 by nobody. All rights reversed.</p>
    </footer>
  </body>
</html>
```



### Non Semantic Wrappers

Wrappers(elements) that have no semantic meaning, the elements don't have any meaning. Used to group content together without adding any semantic meaning to the content group. Use to style or add logic with CSS or JavaScript.

[`<span>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/span) inline element that should only be used if you can't think of a better semantic text element to wrap your content, or don't want to add any specific meaning.
attributes:
* class: The class attribute **specifies one or more classnames for an element**. The class attribute is mostly used to point to a class in a style sheet. However, it can also be used by a JavaScript (via the HTML DOM) to make changes to HTML elements with a specified class.
```html
<p>
  The King walked drunkenly back to his room at 01:00, the beer doing nothing to
  aid him as he staggered through the door.
  <span class="editor-note">
    [Editor's note: At this point in the play, the lights should be down low].
  </span>
</p>
```

[`<div>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/div) is a block level non-semantic element, which you should only use if you can't think of a better semantic block element to use, or don't want to add any specific meaning.
```html
<div class="shopping-cart">
  <h2>Shopping cart</h2>
  <ul>
    <li>
      <p>
        <a href=""><strong>Silver earrings</strong></a>: $99.95.
      </p>
      <img src="../products/3333-0985/thumb.png" alt="Silver earrings" />
    </li>
    <li>…</li>
  </ul>
  <p>Total cost: $237.89</p>
</div>
```

This isn't really an `<aside>`, as it doesn't necessarily relate to the main content of the page (you want it viewable from anywhere). It doesn't even particularly warrant using a `<section>`, as it isn't part of the main content of the page. So a `<div>` is fine in this case. We've included a heading as a signpost to aid screen reader users in finding it.

Divs are so convenient to use that it's easy to use them too much. As they carry no semantic value, they just clutter your HTML code.

You should only use `span` and `div` if the content that you want to wrap up cannot have the sematic meaning of any sematic element, meaning that the logic behind the sematic element does not/cannot apply to this element.




### Line Breaks, Horizontal Rules

`<br>` (break rule): breaks line of a `<p>`'s content for example:
```html
<p>
  There once was a man named O'Dell<br />
  Who loved to write HTML<br />
  But his structure was bad, his semantics were sad<br />
  and his markup didn't read very well.
</p>
```

`<hr />` (horizontal rule): creates a visible horizontal line in your webpage. Semantic meaning: used to divide webpages into different themes.