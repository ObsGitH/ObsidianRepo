
# [Media assets and licensing](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML#media_assets_and_licensing)

This chapter can definetely be resumed by the way.

Images (and other media asset types) you find on the web are released under various license types. Before you use an image on a site you are building, ensure you own it, have permission to use it, or comply with the owner's licensing conditions.

### [Understanding license types](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML#understanding_license_types)

Let's look at some common categories of licenses you are likely to find on the web.

#### All rights reserved

Creators of original work such as songs, books, or software often release their work under closed copyright protection. This means that, by default, they (or their publisher) have exclusive rights to use (for example, display or distribute) their work. If you want to use copyrighted images with an _all rights reserved_ license, you need to do one of the following:

- Obtain explicit, written permission from the copyright holder.
- Pay a license fee to use them. This can be a one-time fee for unlimited use ("royalty-free"), or it might be "rights-managed", in which case you might have to pay specific fees per use by time slot, geographic region, industry or media type, etc.
- Limit your uses to those that would be considered [fair use](https://fairuse.stanford.edu/overview/fair-use/what-is-fair-use/) or [fair dealing](https://copyrightservice.co.uk/copyright/p27_work_of_others) in your jurisdiction.

Authors are not required to include a copyright notice or license terms with their work. Copyright exists automatically in an original work of authorship once it is created in a tangible medium. So if you find an image online and there are no copyright notices or license terms, the safest course is to assume it is protected by copyright with all rights reserved.

#### Permissive

If the image is released under a permissive license, such as [MIT](https://mit-license.org/), [BSD](https://opensource.org/license/BSD-3-clause/), or a suitable [Creative Commons (CC) license](https://creativecommons.org/choose/), you do not need to pay a license fee or seek permission to use it. Still, there are various licensing conditions you will have to fulfill, which vary by license.

For example, you might have to:

- Provide a link to the original source of the image and credit its creator.
- Indicate whether any changes were made to it.
- Share any derivative works created using the image under the same license as the original.
- Not share any derivative works at all.
- Not use the image in any commercial work.
- Include a copy of the license along with any release that uses the image.

You should consult the applicable license for the specific terms you will need to follow.

**Note:** You may come across the term "copyleft" in the context of permissive licenses. Copyleft licenses (such as the [GNU General Public License (GPL)](https://www.gnu.org/licenses/gpl-3.0.en.html) or "Share Alike" Creative Commons licenses) stipulate that derivative works need to be released under the same license as the original.

Copyleft licenses are prominent in the software world. The basic idea is that a new project built with the code of a copyleft-licensed project (this is known as a "fork" of the original software) will also need to be licensed under the same copyleft license. This ensures that the source code of the new project will also be made available for others to study and modify. Note that, in general, licenses that were drafted for software, such as the GPL, are not considered to be good licenses for non-software works as they were not drafted with non-software works in mind.

Explore the links provided earlier in this section to read about the different license types and the kinds of conditions they specify.

#### Public domain/CC0

Work released into the public domain is sometimes referred to as "no rights reserved" — no copyright applies to it, and it can be used without permission and without having to fulfill any licensing conditions. Work can end up in the public domain by various means such as expiration of copyright, or specific waiving of rights.

One of the most effective ways to place work in the public domain is to license it under [CC0](https://creativecommons.org/share-your-work/public-domain/cc0/), a specific creative commons license that provides a clear and unambiguous legal tool for this purpose.

When using public domain images, obtain proof that the image is in the public domain and keep the proof for your records. For example, take a screenshot of the original source with the licensing status clearly displayed, and consider adding a page to your website with a list of the images acquired along with their license requirements.

### [Searching for permissively-licensed images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML#searching_for_permissively-licensed_images)

You can find permissive-licensed images for your projects using an image search engine or directly from image repositories.

Search for images using a description of the image you are seeking along with relevant licensing terms. For example, when searching for "yellow dinosaur" add "public domain images", "public domain image library", "open licensed images", or similar terms to the search query.

Some search engines have tools to help you find images with permissive licenses. For example, when using Google, go to the "Images" tab to search for images, then click "Tools". There is a "Usage Rights" dropdown in the resulting toolbar where you can choose to search specifically for images under creative commons licenses.

Image repository sites, such as [Flickr](https://flickr.com/), [ShutterStock](https://www.shutterstock.com/), and [Pixabay](https://pixabay.com/), have search options to allow you to search just for permissively-licensed images. Some sites exclusively distribute permissively-licensed images and icons, such as [Picryl](https://picryl.com/) and [The Noun Project](https://thenounproject.com/).

Complying with the license the image has been released under is a matter of finding the license details, reading the license or instruction page provided by the source, and then following those instructions. Reputable image repositories make their license conditions clear and easy to find.
# Images

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth"
  width="400"
  height="341" />
```

`<img />` is a void element used to apply image resources/files to your document.
attributes:
* `src`: path of the image file
* `width` and `height`: values should be equal to the height and the width of the image. Useful because it allows the webpage to calculate the space required for the image before the image is loaded. Should be always provided when possible.
* `alt`: value should be a text that describes the image. Below are 4 reasons to use `alt`:
- **Decoration.** You should use [CSS background images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML#css_background_images) for decorative images, but if you must use HTML, add a blank `alt=""`. If the image isn't part of the content, a screen reader shouldn't waste time reading it.
- **Content.** If your image provides significant information, provide the same information in a _brief_ `alt` text – or even better, in the main text which everybody can see. Don't write redundant `alt` text. How annoying would it be for a sighted user if all paragraphs were written twice in the main content? If the image is described adequately by the main text body, you can just use `alt=""`.
- **Link.** If you put an image inside [`<a>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) tags, to turn an image into a link, you still must provide [accessible link text](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks#use_clear_link_wording). In such cases you may, either, write it inside the same `<a>` element, or inside the image's `alt` attribute – whichever works best in your case.
- **Text.** You should not put your text into images. If your main heading needs a drop shadow, for example, [use CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/text-shadow) for that rather than putting the text into an image. However, If you _really can't avoid doing this_, you should supply the text inside the `alt` attribute.


To create annotations to the image you need the `<figure>` and `<figcaption>` (figure caption). wrap the image up in the `<figure>` and write the image's annotation as the content of a `<figcaption>` that will also be wrapped up inside `<figures>`:
```html
<figure>
  <img
    src="images/dinosaur.jpg"
    alt="The head and torso of a dinosaur skeleton;
            it has a large head with long sharp teeth"
    width="400"
    height="341" />

  <figcaption>
    A T-Rex on display in the Manchester University Museum.
  </figcaption>
</figure>

```

A `figure `could be several images, a code snippet, audio, video, equations, a table, or something else. `figure` traits:
1. Has compact/immediate meaning (simple), 2. could go anywhere in the page (independent), 3. Provides information that supports the main text (supportive).

## ## [CSS background images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML#css_background_images)

The CSS [`background-image`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-image) property, and the other `background-*` properties, are used to control background image placement. For example, to place a background image on every paragraph on a page, you could do this:

```css
p {
  background-image: url("images/dinosaur.jpg");
}
```

CSS background images are for decoration only. If you just want to add something pretty to your page to enhance the visuals, this is fine. Though, such images have no semantic meaning at all. They can't have any text equivalents, are invisible to screen readers, and so on. 

 If an image has meaning, in terms of your content, you should use an HTML image. If an image is purely decoration, you should use CSS background images.

## [Other graphics on the web](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML#other_graphics_on_the_web)

The browser offers ways of creating 2D and 3D graphics with code, as well as including video from uploaded files or live streamed from a user's camera. Here are links to articles that provide insight into these more advanced graphics topics:

[Canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)

The [`<canvas>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) element provides APIs to draw 2D graphics using JavaScript.

[SVG](https://developer.mozilla.org/en-US/docs/Web/SVG)

Scalable Vector Graphics (SVG) lets you use lines, curves, and other geometric shapes to render 2D graphics. With vectors, you can create images that scale cleanly to any size.

[WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API)

The WebGL API guide will get your started with WebGL, the 3D graphics API for the Web that lets you use standard OpenGL ES in web content.

[Using HTML audio and video](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Video_and_audio_content)

Just like `<img>`, you can use HTML to embed [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) and [`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio) into a web page and control its playback.

[WebRTC](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API)

The RTC in WebRTC stands for Real-Time Communications, a technology that enables audio/video streaming and data sharing between browser clients (peers).



# Video and Audio

**Note:**  [YouTube](https://www.youtube.com/), [Dailymotion](https://www.dailymotion.com/), and [Vimeo](https://vimeo.com/), and online audio providers like [Soundcloud](https://soundcloud.com/)offer a convenient, easy way to host and consume videos, so you don't have to worry about the enormous bandwidth consumption. They offer ready-made code for embedding video/audio in your webpages so you can avoid some of the difficulties we discuss here.

