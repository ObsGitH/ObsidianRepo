
1. **What is your website about?** Do you like dogs, New York, or Pac-Man?
2. **What information are you presenting on the subject?** Write a title and a few paragraphs and think of an image you'd like to show on your page.
3. **What does your website look like,** in simple high-level terms? What's the background color? What kind of font is appropriate: formal, cartoony, bold and loud, subtle?

Then grab a piece of paper and draw a sketch(shitty drawing) of how you want your website to look like

Next you need to think about what assets you are going to include in your website: Text, Font of the texts, Images?, Videos?, What is going to be the theme color of your website?

Next after determining the assets you need to think about in what way you want them to be displayed in your website:
different html pages? In the navigation menu? In different sections of the same page? What image will I use for the website's icon? etc.

If your are actually going to release the website officially you need to be careful about copyright. Only use forms of media that belong to the public domain or media that have copyrights that allow to use that media. Otherwise you can be fucked by a lawsuit.

You should put the files of a project inside the same directory. Think of the directory as the website in it's totality. You should also put the website directory inside another directory, this directory should be where all your websites are stored.

When naming files for the website, every character should be lowercase and separated by _ (underscore) or - (hyphen).Why?
Why only lowercase: Many computers, particularly web servers, are case-sensitive. So for example, if you put an image on your website at `test-site/MyImage.jpg` and then in a different file you try to invoke the image as `test-site/myimage.jpg`, it may not work.

Why no spaces: Browsers, web servers, and programming languages do not handle spaces consistently. For example, if you use spaces in your filename, some systems may treat the filename as two filenames. Some servers will replace the spaces in your filenames with "%20" (the character code for spaces in URLs), resulting in all your links being broken. It's better to separate words with hyphens, rather than underscores: `my-file.html` vs. `my_file.html`.

How the website directory should look:
1. The index.html file: where the home page of the website will be written.
2. The `MultiMedia` Directory: Where all types of media: audio, video, images, etc will be stored.
3. The Scripts Directory: Where the logic of the website will be written with `.js` files.
4. The Styles Directory: Where the CSS style folder's will be stored.


You are going to use the `FilePath` of the files to use them inside the folder's. (Pretty Obvious shit).
- To link to a target file in the same directory as the invoking HTML file, just use the filename, e.g. `my-image.jpg`.
- To reference a file in a subdirectory, write the directory name in front of the path, plus a forward slash, e.g. `subdirectory/my-image.jpg`.
- To link to a target file in the directory **above** the invoking HTML file, write two dots. So for example, if `index.html` was inside a subfolder of `test-site` and `my-image.jpg` was inside `test-site`, you could reference `my-image.jpg` from `index.html` using `../my-image.jpg`.

Always use relative paths when hardcoding the `FilePath` in the files. This way you can move your website's directory anywhere without making your website break.

Also after the webpages (`.html` files) are already finished. Think about the internal link structure that you are going to use in your website.

# Common Web Site Layouts

[Header](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts#header)

Visible at the top of every page on the site. Contains information relevant to all pages (like site name or logo) and an easy-to-use navigation system.

[Main content](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts#main_content)

The biggest region, containing content unique to the current page.

[Stuff on the side](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts#stuff_on_the_side)

1) Information complementing the main content; 2) information shared among a subset of pages; 3) alternative navigation system. In fact, everything not absolutely required by the page's main content.

[Footer](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts#footer)

Visible at the bottom of every page on the site. Like the header, contains less prominent global information like legal notices or contact info.

These elements are quite common in all form factors, but they can be laid out different ways. Here are some examples (**1** represents header, **2** footer; **A** main content; **B1, B2** things on the side):

**1-column layout**. Especially important for mobile browsers so you don't clutter the small screen up.

![Example of a 1 column layout: Main on top and asides stacked beneath it.](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts/1-col-layout.png)

**2-column layout**. Often used to target tablets, since they have medium-size screens.

![Example of a basic 2 column layout: One aside on the left column, and main on the right column.](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts/2-col-layout-right.png)![Example of a basic 2 column layout: One aside on the right column, and main on the left column.](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts/2-col-layout-left.png)

**3-column layouts**. Only suitable for desktops with big screens. (Even many desktop-users prefer viewing things in small windows rather than fullscreen.)

![Example of a basic 3 column layout: Aside on the left and right column, Main on the middle column.](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts/3-col-layout.png)![Another example of a 3 column layout: Aside side by side on the left, Main on the right column.](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts/3-col-layout-alt.png)![Another example of a 3 column layout: Aside side by side on the right, Main on the left column.](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts/3-col-layout-alt2.png)

The real fun begins when you start mixing them all together:

![Example of mixed layout: Main on top and asides beneath it side by side.](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts/1-col-layout-alt.png)![Example of a mixed layout: Main on the left column and asides stack on top of each other on the right column](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts/2-col-layout-left-alt.png)![Example of a mixed layout: one aside on the left column and main in the right column with an aside beneath main.](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts/2-col-layout-mix.png)![Example of a mixed layout: Main on the left of the first row and one aside on the right of that same row, a second aside covering the whole second row.](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Design_and_accessibility/Common_web_layouts/2-col-layout-mix-alt.png)…

These are just examples and you're quite free to lay things out as you want. You may notice that, while the content can move around on the screen, we always keep the header (1) on top and the footer (2) at the bottom. Also, the main content (A) matters most, so give it most of the space.

These are rules of thumb you can draw on. There are complex designs and exceptions, of course. In other articles we'll discuss how to design responsive sites (sites that change depending on the screen size) and sites whose layouts vary between pages. For now, it's best to keep your layout consistent throughout your site.
# What If You Are Going To Publish The Website?

Most people choose to buy web hosting and a domain name:

- Web hosting is rented file space on a hosting company's [web server](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_web_server). You put website files on the web server. The web server provides website content to website visitors.
- A [domain name](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_domain_name) is the unique address where people find your website, such as `https://www.mozilla.org` or `https://www.bbc.co.uk`. You can rent your domain name for as many years as you want from a **domain registrar**.

Many professional websites go online this way because it gives you more control  of the content and appearance of the website.

you will need a [File Transfer Protocol (FTP)](https://developer.mozilla.org/en-US/docs/Glossary/FTP) program (see [How much does it cost: software](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Tools_and_setup/How_much_does_it_cost#software) for more details) to actually transfer the website files over to the server. FTP programs vary widely, but generally, you have to connect to your web server using details provided by your hosting company (typically username, password, hostname). Then the program shows you your local files and the web server's files in two windows, and provides a way for you to transfer files back and forth.

Tips For Hosting And Domains:
1. To find hosting companies and registrars, just search for "web hosting" and "domain names". All registrars will have a feature to allow you to check if the domain name you want is available.
2. - Your home or office [internet service provider](https://developer.mozilla.org/en-US/docs/Glossary/ISP) may provide some limited hosting for a small website. The available feature set will be limited, but it might be perfect for your first experiments.
3. You also have free services.
4. A lot of companies providing hosting and domain.
- [GitHub](https://github.com/) is a "social coding" site. It allows you to upload code repositories for storage in the [Git](https://git-scm.com/) **version control system.** You can then collaborate on code projects, and the system is open-source by default, meaning that anyone in the world can find your GitHub code, use it, learn from it, and improve on it. GitHub has a very useful feature called [GitHub Pages](https://pages.github.com/), which allows you to expose website code live on the web.
- [Google App Engine](https://cloud.google.com/appengine/) is a powerful platform that lets you build and run applications on Google's infrastructure — whether you need to build a multi-tiered web application from scratch or host a static website. See [How do you host your website on Google App Engine?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Tools_and_setup/How_do_you_host_your_website_on_Google_App_Engine) for more information.

These last 2 options are usually free, but you may outgrow the limited feature-set.

IDE's:
Most programmer think that VS Code is the best for HTML, CSS and JavaScript.

Publishing Website in GitHub:

1. You need to [create a repository](https://github.com/new) to store files.
2. On this page, in the _Repository name_ box, enter _username_.github.io, where _username_ is your username. For example, our friend Bob Smith would enter _bobsmith.github.io_. Check the "_Initialize this repository with a README"_ box. Then click _Create repository_.![A sample of a GitHub repository page](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/Publishing_your_website/github-create-repo.png)
3. Drag and drop the content of your website folder into your repository. Then click _Commit changes_.
    
    **Note:** Make sure your folder has an `index.html` file.
    
4. Navigate your browser to _username_.github.io to see your website online. For example, for the username `_chrisdavidmills_`, go to [_chrisdavidmills_.github.io](https://chrisdavidmills.github.io/).
    
    **Note:** It may take a few minutes for your website to go live. If your website does not display immediately, wait a few minutes. Try again.




# [Planning a simple website](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure#planning_a_simple_website)

Once you've planned out the structure of a simple webpage, the next logical step is to try to work out what content you want to put on a whole website, what pages you need, and how they should be arranged and link to one another for the best possible user experience. This is called [Information architecture](https://developer.mozilla.org/en-US/docs/Glossary/Information_architecture). In a large, complex website, a lot of planning can go into this process, but for a simple website of a few pages, this can be fairly simple, and fun!

1. Bear in mind that you'll have a few elements common to most (if not all) pages — such as the navigation menu, and the footer content. If your site is for a business, for example, it's a good idea to have your contact information available in the footer on each page. Note down what you want to have common to every page.![the common features of the travel site to go on every page: title and logo, contact, copyright, terms and conditions, language chooser, accessibility policy](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure/common-features.png)
2. Next, draw a rough sketch of what you might want the structure of each page to look like (it might look like our simple website above). Note what each block is going to be.![A simple diagram of a sample site structure, with a header, main content area, two optional sidebars, and footer](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure/site-structure.png)
3. Now, brainstorm all the other (not common to every page) content you want to have on your website — write a big list down.![A long list of all the features that we could put on our travel site, from searching, to special offers and country-specific info](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure/feature-list.png)
4. Next, try to sort all these content items into groups, to give you an idea of what parts might live together on different pages. This is very similar to a technique called [Card sorting](https://developer.mozilla.org/en-US/docs/Glossary/Card_sorting).![The items that should appear on a holiday site sorted into 5 categories: Search, Specials, Country-specific info, Search results, and Buy things](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure/card-sorting.png)
5. Now try to sketch a rough sitemap — have a bubble for each page on your site, and draw lines to show the typical workflow between pages. The homepage will probably be in the center, and link to most if not all of the others; most of the pages in a small site should be available from the main navigation, although there are exceptions. You might also want to include notes about how things might be presented.![A map of the site showing the homepage, country page, search results, specials page, checkout, and buy page](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure/site-map.png)

