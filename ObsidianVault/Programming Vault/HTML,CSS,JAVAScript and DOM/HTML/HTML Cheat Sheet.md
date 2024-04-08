
# 1. Elements or Tags
1. **Document Structure:**
    
    - `<html>`
    - `<head>`
        - `<title>`
        - `<meta>`
        - `<link>`
        - `<style>`
        - `<script>`
        - `<base>`
    - `<body>`
1. **Sections and Headings:**
    
    - `<header>`
    - `<nav>`
    - `<main>`
    - `<section>`
    - `<article>`
    - `<aside>`
    - `<footer>`
    - `<h1>`, `<h2>`, ..., `<h6>`
3. **Text Content:**
    
    - `<p>`
    - `<a>`
    - `<span>`
    - `<strong>`, `<b>`
    - `<em>`, `<i>`
    - `<u>`
    - `<br>`
    - `<hr>`
    - - `<wbr>`
1. **Lists:**
    
    - `<ul>` or `<menu>`
        - `<li>`
    - `<ol>`
        - `<li>`
    - `<dl>`
        - `<dt>`
        - `<dd>`
5. **Forms:**
		
    - `<form>`
        - `<input>`
        - `<label>`
        - `<textarea>`
        - `<select>`
            - `<option>`
        - `<button>`
	or
	- `<search>`
		- `<form>`
		- so on...
1. **Tables:**
    
    - `<table>`
	    - `<caption>`
	    - `<colgroup>`
		    - `<col>`
        - `<thead>`, `<tbody>`, `<tfoot>`
            - `<tr>`
                - `<th>`
                - `<td>`
            
1. **Media:**
    
    - `<img>`
	    - `<source />`
	    - `<map>`
		    - `<area>`
    - `<audio>`
	    - `<source />`
    - `<video>`
	    - `<source />`
	    - `<track />`
1. **Interactive Elements:**
    
    - `<button>`
    - `<a>`
    - `<input>`
9. **Semantic Elements:**
    
    - `<mark>`
    - `<small>`
    - `<strong>`, `<b>`
    - `<em>`, `<i>`
    - `<abbr>`
    - `<blockquote>`
    - `<q>`
    - `<cite>`
    - `<code>`
    - `<pre>`
    - `<del>`
    - `<ins>`
    - `<sub>`
    - `<sup>`
    - `<time>`
10. **Inline Text Semantics:**
    
    - `<a>`
    - `<abbr>`
    - `<b>`
    - `<bdi>`
	- `<bdo>`
    - `<cite>`
    - `<code>`
    - `<em>`
    - `<i>`
    - `<mark>`
    - `<small>`
    - `<strong>`
    - `<sub>`
    - `<sup>`
    - `<dfn>`
    - `<kbd>`
    - `<samp>`
    -  `<s>`
1. **Grouping Content:**
    
    - `<div>`
    - `<span>`
    - `<ul>`, `<ol>`, `<dl>`
    - `<figure>`
	    - `<img>`
	    - `<figcaption>`
    - `<blockquote>`, `<q>`
    - `<address>`
    - `<fieldset>`
	    - `<legend>`
    - `<hgroup>`
    - `<picture>`
	    - `<source>`
1. **Scripting:**
    
    - `<script>`
    - `<noscript>`
    - `<template>` (not really but there is no other place to put it.)
1. **Embedded Content:**
    
    - `<iframe>`
    - `<object>`
    - `<embed>`
    - `<canvas>`
    - `<svg>`
    - `<object>`
1. **Forms and Input and Output Elements:**
    
    - `<form>`
    - `<input>`
    - `<textarea>`
    - `<select>`
        - `<option>`
        - `<optgroup>`
    - `<output>`
1. **Interactive Elements:**
    
    - `<a>`
    - `<button>`
    - `<details>`
        - `<summary>`
    - `<label>`
        - `<input>`
	        - `<datalist>`
		        - `<option>`
        - `<select>`
        - `<textarea>`
    - `<dialog>
1. Ruby:
- `<rp>`
- `<rt>
- `<ruby>`

Anti-Social Elements:

- `<meter>`
- `<slot>`

Super-Social Elements:
- `data` - gives any element that is being wrapped up in this tag a value.

# 2. Global Attributes

- `acesskey` -> uma tecla pra vc criar um atalho igual a  ALT + tecla.
- `autocapitalize` -> allows you to capitalize sentences, words, every single character or nothing automatically.
- `autofocus` -> makes the element be the focused element when the page loads. Only one element in a html document can have that attribute at a time.
- `class `-> defines a class for the element.
- `contenteditable` -> allows client to edit the html element.
- `dir` -> changes the direction of the text inside the element.
- `draggable` -> allow the user to drag the element.
- `enterkeyhint` -> defines action icon/label for the enter key on virtual keyboards.
- `hidden` -> hides the element.
- `id` -> defines the element id so the element can be referenced in other elements. The id can be passed as a value to other atributes.
- `inert` -> prevents the click and focus events from being raised and also hides the element in the acessability tree making the element hidden to people who have disabilities.
- `inputmode` -> enumerated attribute where you can define the specific type of data that will be accepted by the element. Used in `<input>` and any other element in `contenteditable` mode.
- `is` -> used to make the HTML document aware that this is a custom element. The value that you pass is the name of the user-defined class of said element. This attribute requires that the custom element has been defined (with JavaScript) and that said custom-element is a child class of the standard built-in element that the attribute is being applied to. [Know More](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/is)
- `lang` -> defines the language of the element. Every element that is not in the main language of the document (defined in: `<html lang="">`) needs to apply this attribute and pass the language of said element as the value.
- `nonce` -> Number used once. Determines whether or not a given fetch will be allowed to proceed for a given element.[Know More](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce)
- `part` -> contains a space separated list of the element's part names. CSS can style said parts using ::part(part name) pseudo-element.
- `popover` -> to make that element a popover element. Popover elements are hidden via `display: none` until opened via an invoking/control element (i.e. a `<button>` or `<input type="button">` with a [`popovertarget`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button#popovertarget) attribute) or a [`HTMLElement.showPopover()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/showPopover) call. [Know More](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/popover)
- `spellcheck` -> enable or disable spellcheck for the text of said element. Used mostly in `input` and `textarea`  elements and the like.
- `style` -> used to generate very simple CSS style to that element.
- `tabindex` -> defines order of elements that will become focused when pressing the TAB key. The element with `tabindex="1"` will be the first, the element with the value 2 will be second and so on.
- `title` -> Should contain a text(html)/string(C#/JavaScript) that represents information that will tell you what the element is about.\
- `translate` -> If you want the element to be translatable by the browser automatic translation give this attribute the value: yes. Otherwise give it: no. Yes by default.
## Other Global Attributes

### DOM related global attributes
- `data-*` -> custom data attributes that allow HTML and DOM to exchange proprietary information.
- `exportparts` -> allows you to select and style elements existing in nested [shadow trees](https://developer.mozilla.org/en-US/docs/Glossary/Shadow_tree), by exporting their `part` names.
- Assigns a slot in a [shadow DOM](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM) shadow tree to an element: An element with a `slot` attribute is assigned to the slot created by the [`<slot>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot) element whose [`name`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot#name) attribute's value matches that `slot` attribute's value. [Know More](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/slot)


## Global Attributes that I Do Not Understand
- `itemid`
- `itemprop`
- `itemref`
- `itemscope`
- `itemtype`

# Non Global Attributes

# 1. [accept](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/accept)
    
# 2. [autocomplete](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete)

# 3. _[capture](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/capture)_

# 4. [crossorigin](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/crossorigin)
    
# 5. [dirname](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/dirname)

# 6. [disabled](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/disabled)

# 7. [elementtiming](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/elementtiming)
    
# 8. [for](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/for)
    
# 9. [max](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/max)

# 10. [maxlength](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/maxlength)
 
# 11. [min](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/min)
    
# 12. [minlength](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/minlength)
    
# 13. [multiple](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/multiple)
    
# 14. [pattern](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/pattern)
    
# 15. [placeholder](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/placeholder)
    
# 16. [readonly](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/readonly)
    
# 17. [rel](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel)
    
# 18. [required](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/required)
    
# 19. [size](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/size)
    
# 20. [step](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/step)


