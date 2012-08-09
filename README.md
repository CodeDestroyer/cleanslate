About this project
-------------------

Dharmafly Docs is a Github Pages templating system. It allows project members to create a website for their Dharmafly projects.


About this branch
-----------------

This is the development branch. If you're looking to create a website for your Dharmafly project, see the main [README] (https://github.com/dharmafly/dharmafly-docs)

If you're looking to update the general Dharmafly Docs template, carry out all development work on Dharmafly Docs in this branch.

This branch is also the working code for the [Dharmafly Docs project website] (http://dharmafly.github.com/dharmafly-docs/), which contains the styleguide.

The [`master` branch] (https://github.com/dharmafly/dharmafly-docs) contains an empty template, reflecting the latest code and should be used by project developers to generate new project websites.

Before starting
------------------

Before updating the Dharmafly Docs website, it is assumed you're familiar with creating Dharmafly Docs instances and have read the main [Dharmafly Docs README](https://github.com/dharmafly/dharmafly-docs/)

Updating an existing project
-----------------------------

Full documentation can be found on the main [README] (https://github.com/dharmafly/dharmafly-docs#updating-an-existing-project)


How Can I set up a new Dharmafly project website?
----------------------------

Full documentation can be found on the main [README] (https://github.com/dharmafly/dharmafly-docs#https://github.com/dharmafly/dharmafly-docs#how-can-i-set-up-a-new-dharmafly-project-website)

Getting started
===================

How do I update the Dharmafly Docs project itself?
--------------------------------------

This is the development branch, you should carry out all work in this branch. 

First clone this repository (`git clone git@github.com:dharmafly/dharmafly-docs.git`) and switch to this branch (`git checkout gh-pages`.)

You should make your changes within `gh-pages` as this branch contains example posts that will enable you to test your changes.

Once pushed to this branch (`git push origin gh-pages`), Github will automatically regenerate the [Dharmafly Docs project website] (http://dharmafly.github.com/dharmafly-docs/).

The `master` branch contains an empty instance of Dharmafly Docs. You will need to update this branch, to allow other projects to update, or create new instances.

To update the [`master` branch] (https://github.com/dharmafly/dharmafly-docs), switch to the master branch (`git checkout master`), then pull the changes made in this branch (`git pull origin gh-pages`). This may result in a merge conflict with the `README.md` (as the content on the `master` branch is different to this README). The temporary fix for this is to copy the current `master` README from https://github.com/dharmafly/dharmafly-docs/blob/master/README.md.

If you've added any files to the `assets` directory, or updated any posts in the `_posts` directory, these will be pulled-in to the `master` branch. As you won't really want instances of Dharmafly Docs to contain all the assets (psd files, master pngs, etc), or any of the Dharmafly Docs posts, you should delete these directories before commiting.

Once happy with your merge to the `master` branch,  `add`, `commit` the `git push origin master`.

The site structure
------------------------

The main frameworks (`<head>`,`<body>` tags, and so on) are within the `_layouts` folder. The `default.html` layout is used currently on all pages.

`default.html` contains a [liquid](http://liquidmarkup.org/) tag for the variable `{{ content }}`, a liquid reserved word, used for the content of 'this' page. So if the user has gone to the site home page, then `index.html` will be the `{{ content }}`. If you've gone to `/reference/`, then `/reference/index.html` will be the `{{ content }}`

Within each `index.html`, the content of the page is constructed  via liquid `{{ for }}` loops over the content of the posts in the `_posts` directory (see '[templating using liquid](#templating-using-liquid)' below). 

Templating using liquid
----------------------

The templating language for github pages is [Liquid](http://liquidmarkup.org/).

Within any HTML, CSS, JavaScript page on the site any liquid tags are parsed by [Jekyll](https://github.com/mojombo/jekyll/) (the templating engine).

### Pre-defined global variables

The key [predefined Jekyll global variables](https://github.com/mojombo/jekyll/wiki/Template-Data) are 
- `site`, which contains global properties for the site (e.g. those [specified within `_cofig.yml`](https://github.com/dharmafly/dharmafly-docs/#site-variables)), 
- `post`, which contains details for each post, 
- `categories`/`category` which group posts 
- `layout`, mentioned [above](#the-site-structure) and 
- `page`, used to refer to the current page (as opposed to post) within the layout page, `_layouts/default.html`.

### Posts and categories

Any file within the site can be parsed by Jekyll. Each Jekyll parseable page has a [YAML front matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter) section.

Within the `index.html` pages, Jekyll will iterate over the categories that are declared within each post files to construct the page.
    
Each post within the `_posts` directory requires a `category` property to be specified within the front matter (currently only `reference` and `overview` are used). 

We use this category value as a way of choosing which posts will be displayed on which page - this is done using the loop:

    {% for post in overview reversed %} (where post is an alias for site.categories.overview)

Adding new pages
-------------------

There is no facility to do this easily - [a ticket exists](https://github.com/dharmafly/dharmafly-docs/issues/1) to add new pages to the site based on the directory structure of the `master` branch's `docs` directory.

To do this manually, you would need to:

1. Copy and rename the `reference` directory to create another page.
2. Replace `reference` with your new page category
3. Ensure all posts to be displayed on this page have your new page category in their front matter.

To ensure this page is now linked to from the home and other pages, you will need to update the `_config.yml` with a new `section`

Updating the CSS
-----------------

The site CSS is in `/_includes/global.css`. 

The site colour theme files are stored within the `/css/` directory. 

Each theme file contains [YAML front matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter) describing the colours and assets used for that theme and a liquid `{% include global.css %}`. Jekyll will populate the variables within `/_includes/global.css` using the values set within the theme file.

The site theme is specified per site within the `_config.yml`. The CSS file is added to the page HTML within `/_includes/default.html`. 

### Creating new themes / colour schemes

To add a new theme:

1. Create a new theme CSS file - copy an existing theme file and update the values in the front matter. The theme file comprises a YAML front matter section and a line including the `global.css` file.
2. Add a new main SVG asset (optional). The SVG asset could be a new SVG file, or one of the existing SVG elements could be reused. These are stored within the `/css/svg` directory. Specify the new SVG file in your new theme file front matter by updateing the `svg_asset` property
3. Add a favicon to the /img/ directory. The favicons are named to match the theme, so `ocean-favicon.ico` is used in the `ocean` theme.

#### Non-colour updates to themes

- `badge_overlay` and `badge_border` are url encoded (this can be achieved by using javascript `escape()`) in order to be placed within [SVG elements](#svg-how-and-where-it-s-used). `badge-overlay` is `rgba`.
- `svg_asset` specifies the main SVG element used on the page. It will refer to your main SVG file within `/css/svg`.
- `svg_title_filter` and `svg_title_rotation` allow you to apply a filter and rotation to the `svg_asset` within the main title area.
- The size and rotation of the main SVG element as applied to the bottom left of the content area can be updated using `svg_asset_size` and `svg_asset_rotation`.
- There are two scaled versions of the main SVG element above the `QUOTE` (as specified in `_config.yml`), if present. 

```
quote_svg_left_transform: none
quote_svg_right_transform: scaleX(-1)
quote_svg_left_pos: "50%"
quote_svg_right_pos: "49%"
```    

These attributes allow you to position these two elements on the page. The quote_svg_right_transform and quote_svg_left_transform allow you to flip or rotate these SVG elements.

### Updating SVG elements

As noted above, a new SVG element for a theme can be added as a file, then specified in the theme file.

If the SVG elements on the page for all themes require updating (the aside badges 'stem', the subnav underlines, the 'show subnav' icon), then the changes need to be made in `/_includes/global.css`.

These SVG elements are declared as data URIs with an `image/svg+xml` mimetype. In order to render successfully in Firefox, they need to be escaped (Chrome renders the SVG when plain text in the data URI). 

To view a readable SVG, copy the the encode URI from the comma to the final quote, so in :

    background: url('data:image/svg+xml,%3Csvg%20width%3D%2278%22%20height%3D%2278%22%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%3E%3Ccircle%20fill%3D%22{{ page.badge_overlay }}%22%20stroke%3D%22{{ page.badge_border }}%22%20stroke-width%3D%2210%22%20cx%3D%2234%22%20cy%3D%2234%22%20r%3D%2229%22/%3E%3C/svg%3E') no-repeat left top;
    
the string you require is `%3Csvg%20width%3D%2278%22%20height%3D%2278%22%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%3E%3Ccircle%20fill%3D%22{{ page.badge_overlay }}%22%20stroke%3D%22{{ page.badge_border }}%22%20stroke-width%3D%2210%22%20cx%3D%2234%22%20cy%3D%2234%22%20r%3D%2229%22/%3E%3C/svg%3E` this string. 

Within your browser console, run `unescape(<encoded string>)`. you should get an SVG element of the form `<svg width="78" height="78" xmlns="http://www.w3.org/2000/svg"><circle fill="{{ page.badge_overlay }}" stroke="{{ page.content_bg_colour }}" stroke-width="10" cx="34" cy="34" r="29"/></svg>`. In this SVG element, the `{{ page.badge_overlay }}` and `{{ page.content_bg_colour }}` define the colour for the SVG element based on the theme.

You can then edit the SVG element in [an SVG editor](http://svg-edit.googlecode.com/svn/trunk/editor/svg-editor.html). Once happy with the output SVG paste back into the console and `escape()` the string. This can be pasted back into the data URI in `global.css`.


Blocks of code in posts
--------------------------

Blocks of code can be added to posts using code block syntax within markdown posts. These are transformed to `<pre><code>` blocks by Jekyll. 

The code block syntax highlighting and editing used on a site depends on the user's browser and the screenwidth. The initial checks are made by `/javascript/main.js` and for lower IE browsers and smaller screen widths, the lightweight `hijs.js` is loaded for syntax highlighting. In other cases ACE editor is loaded for syntax highlighting and code editing (if required).

In both cases `/javascript/demo.js` is loaded to handle code execution, if required.

### Code highlighting themes

There are two themes used by Dharmafly Docs.  

The first is used by `hijs.js` for small screens and lower IE versions. The colouring is controlled within the main CSS file, `/_includes/global.css`.

The second colour theme is used by ACE editor, and is set within `demo.js` using ace's `setTheme` method. The themes are stored in `/javascript/ace/theme/`.

### Allowing users to edit and run code inline

If narrow screen or IE, hijs is loaded for syntax highlighting, ace editor is not created and the code execution is performed on the code as added to the post.

In other cases, if there's an `alert` or `$output` string in the code, an ace editor is created, that can be edited and the code executed. If there isn't a read only ace editor is created. Syntax highlighting is performed by ace.

Responsive design
-------------------------------------

### Breakpoints

The CSS is structured smallest width by default. 

The widths are controlled by [CSS media queries](https://developer.mozilla.org/en-US/docs/CSS/Media_queries)

- 340px small breakpoint to fix the content left.
- 35em adds the larger header
- 52em adds the icons on right hand side (moves the `aside` element), adds the subnav


### The subnav

The subnav is the left-hand list of inline links. The event handlers for page resizing are in `/javascript/main.js`.

The subnav is shown by default. If the user resizes the screen to a size where the width of the widest link in the subnav is pushed off-screen, then the function `resetSubnav` adds two classes: the subnav is set `.off-left` and the `.show-subnav` icon is shown in the navigation. Additionally, the subnav is hidden. 

If the user clicks on the show subnav icon, then the subnav is set `off-left show-nav`. So it's technically off screen but visible, as the `.content` area's left position is set to a position based on the width of the subnav.

The animation is done by css transforms set on certain properties of those classes.

### Scrolling

The navigation (at all widths) is set below the page heading (project title). On scroll, if the scroll position is greater than the navigation height, then the navigation is set to 'float' over the content (i.e. `position: fixed`). Visually, the navigation is then fixed to the top of the browser viewport.

The event handlers for scroll events are in `/javascript/main.js`.

Additionally, the subnav needs to be set to float over the content (so it always appears in the top left of the browser viewport, as the page scrolls), so the `resetSubnav` function is called from within `onScroll`. `resetSubnav` will set or get an offset value for the subnav, then set the view state for the subnav.

The `onScroll` function is executed on a throttled interval based on the firing of scroll events.

SVG - how and where it's used
------------------------------

Details in [this post](https://github.com/dharmafly/dharmafly-docs/blob/gh-pages/assets/svg-post/svg%20post.md).