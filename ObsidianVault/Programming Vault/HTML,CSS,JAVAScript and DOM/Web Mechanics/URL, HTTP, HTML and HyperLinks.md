
the three pillars on which the Web stands:

1. [URL](https://developer.mozilla.org/en-US/docs/Glossary/URL), an address system that keeps track of Web documents
2. [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP), a transfer protocol to find documents when given their URLs
3. [HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML), a document format allowing for embedded _hyperlinks_

The Web's original purpose was to provide an easy way to reach, read, and navigate through text documents. Since then, the Web has evolved to provide access to images, videos, and binary data, but these improvements have hardly changed the three pillars.

hyperlinks revolutionized everything. Links can alias any text string with any URL, such that the user can instantly reach the target document by activating the link. You are literally using hyperlinks in this obsidian note.


### [Types of links](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_are_hyperlinks#types_of_links)

[Internal link](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_are_hyperlinks#internal_link)

A link between two webpages, where both webpages belong to the same website, is called an internal link. Without internal links, there's no such thing as a website (unless, of course, it's a one-page website).

[External link](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_are_hyperlinks#external_link)

A link from your webpage to someone else's webpage. Without external links, there is no Web, since the Web is a network of webpages. Use external links to provide information besides the content available through your webpage.

[Incoming links](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_are_hyperlinks#incoming_links)

A link from someone else's webpage to your site. It's the opposite of an external link. Note that you don't have to link back when someone links to your site.

### [Anchors](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_are_hyperlinks#anchors)

Most links tie two webpages together. **Anchors** tie two sections of one document together. The website that you are sumarizing right now: mdn web docs uses a lot of anchors in it's webpages.

### Links And Search Engines

Every time search engines crawl a webpage, they index the website by following the links available on the webpage. Search engines not only follow links to discover the various pages of the website, but they also use the link's visible text to determine which search queries are appropriate for reaching the target webpage.

Links influence how readily a search engine will link to your site. The trouble is, it's hard to measure search engines' activities. Companies naturally want their sites to rank highly in search results. We know the following about how search engines determine a site's rank:

- A link's _visible text_ influences which search queries will find a given URL.
- The more _incoming links_ a webpage can boast of, the higher it ranks in search results.
- _External links_ influence the search ranking both of source and target webpages, but it is unclear by how much.

[SEO](https://en.wikipedia.org/wiki/Search_engine_optimization) (search engine optimization) is the study of how to make websites rank highly in search results. Improving a website's use of links is one helpful SEO technique.

# What Is A URL?

**URL** stands for _Uniform Resource Locator_. A URL is nothing more than the address of a given unique resource on the Web. In theory, each valid URL points to a unique resource. Such resources can be an HTML page, a CSS document, an image, etc. In practice, there are some exceptions, the most common being a URL pointing to a resource that no longer exists or that has moved.

As the resource represented by the URL and the URL itself are handled by the Web server, it is up to the owner of the web server to carefully manage that resource and its associated URL.
![[Pasted image 20240212155738.png]]

 _scheme_: indicates the protocol that the browser must use to request the resource (a protocol is a set method for exchanging or transferring data around a computer network). Usually for websites the protocol is HTTPS or HTTP (its unsecured version). Addressing web pages requires one of these two, but browsers also know how to handle other schemes such as `mailto:` (to open a mail client), so don't be surprised if you see other protocols.

_authority_: is separated from the scheme by the character pattern `://` (the colon separates the scheme from the next url part and // indicates that what follow is the authority.). If present the authority includes both the _domain_ (e.g. `www.example.com`) and the _port_ (`80`), separated by a colon:

- The domain indicates which Web server is being requested. Usually this is a [domain name](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_domain_name), but an [IP address](https://developer.mozilla.org/en-US/docs/Glossary/IP_Address) may also be used (but this is rare as it is much less convenient).
- The port indicates the technical "gate" used to access the resources on the web server. It is usually omitted if the web server uses the standard ports of the HTTP protocol (80 for HTTP and 443 for HTTPS) to grant access to its resources. Otherwise it is mandatory.

`/path/to/myfile.html` is the path to the resource on the Web server. In the early days of the Web, a path like this represented a physical file location on the Web server. Nowadays, it is mostly an abstraction handled by Web servers without any physical reality.

`?key1=value1&key2=value2` are extra parameters provided to the Web server. Those parameters are a list of key/value pairs separated with the `&` symbol. The Web server can use those parameters to do extra stuff before returning the resource. Each Web server has its own rules regarding parameters, and the only reliable way to know if a specific Web server is handling parameters is by asking the Web server owner. ? signify that what follows are the parameters.

`#SomewhereInTheDocument` is an anchor to another part of the resource itself. An anchor represents a sort of "bookmark" inside the resource, giving the browser the directions to show the content located at that "bookmarked" spot. # signify that what follows is an anchor.

You will use URL's constantly when creating html documents:

- to create links to other documents with the [`<a>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) element;
- to link a document with its related resources through various elements such as [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) or [`<script>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script);
- to display media such as images (with the [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img) element), videos (with the [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) element), sounds and music (with the [`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio) element), etc.;
- to display other HTML documents with the [`<iframe>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) element.

You should generally only use HTTP and HTTPS URLs in your HTML file's, with few exceptions (one notable one being `data:`; see [Data URLs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URLs)). Using FTP, for example, is not secure and is no longer supported by modern browsers.

### Absolute and Relative URL's

Absolute URL's:  In your browser's address bar, a URL doesn't have any context, so you must provide a full (or _absolute_) URL, like the ones we saw above. You don't need to include the protocol (the browser uses HTTP by default) or the port (which is only required when the targeted Web server is using some unusual port), but all the other parts of the URL are necessary.

Relative URL's: When a URL is used within a document, such as in an HTML page, things are a bit different. Because the browser already has the document's own URL, it can use this information to fill in the missing parts of any URL available inside that document. Relative URL's will take the context as a reference, the context being what is the path URL that we are currently in.


 [_Semantic URLs_](https://en.wikipedia.org/wiki/Semantic_URL)use words with inherent meaning that can be understood by anyone, regardless of their technical know-how.

