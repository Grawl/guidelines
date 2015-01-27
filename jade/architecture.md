# Jade project structure for static website

![](http://i.imgur.com/pBvNagg.png)

Recently I worked with Jade template engine for creating of some HTML files in addition to Sass to create CSS. Jade is the best template engine I ever seen.

And one time when my another project was too large and I think that I need a solid project templates structure to simplify next projects maintenance.

*If you will write an application in Express, you will not ask this question because you already have all this views, partials and other stuff in your framework.*

So we want to generate a pack of HTML files and upload them to our basic web server (like Github Pages) without any language support like Node, Ruby or even PHP.

Now I will share my thoughts about Jade project file structure. *Feel free to fix me if you think I’m wrong.*

## Files

Sample project structure:

- templates
	- layouts
		- base.jade
		- one-column.jade
		- two-columns.jade
	- components
		- header.jade
		- sidebar.jade
		- slider.jade
		- features.jade
	- helpers
		- profile.jade
		- news.jade
		- misc.jade
	- index.jade
	- profile.jade
	- archive.jade
	- article.jade

### `templates` folder

> Folder that contains all your Jade files because all of they are templates: layouts, components, helpers, files with one mixin inside, etc.

> Inside this folder you have to put all your Jade files that will directly create HTML files – your **pages**.

![](http://i.imgur.com/w0XsmHO.png)

Every `page` `extends` one of your `layouts` and contains only calls for your `components` or `helpers`, for example:

`index.jade`

```jade
extends layouts/base
block page
	+slider(data.content.slides)
	+features(data.content.features)
	+text(data.content.about.description)
```

> **Note**: There is no any direct HTML into page code. If you will do it you will break all the things I am talking about.

### `layouts` folder

> Every layout there is place where you collect your partials into a wrapper to dance around your main content.

Every layout extends `base.jade`.

![](http://i.imgur.com/DRfoa7l.png)

Layout can be like this:

`two-columns.jade`

```jade
extends base
block body
	+header
	block page
	+sidebar
	+footer
```

> **Note**: You can use some HTML here to wrap your components into wrappers like `.row` or `.small-full.large-8.columns` to follow your CSS framework guidelines, but I am not recommend this. Better to do this with mixin arguments or something smart.

#### `base.jade` layout

The place where you can do all smart things and place required HTML tags.

Example:

```jade
block variables
	title=data.content.about.name
!doctype html
html
	head
		meta(charset='utf-8')
		title=title
		link(
			rel='stylesheet'
			href='/styles/main.css'
			)
	body
		block main
		script(src='/scripts/main.js')
```

And more, more complicated.

I used this kinds of magic:

- multiple variable blocks: for layout settings, for global settings rewriting, for scripts;
- dynamic title based on page ID with flexible separator;
- connecting Bower with `grunt-wiredep`;
- …and compressing them with `grunt-usemin`;
- and much, much more cool things.

### `components` folder

> Every component is a visible part of your project.

> Components ***must* be reusable**.

> *It is a very importand part of my idea.*

> It can be a whole sidebar or part of it, or universal content slider, or limited news loop.

> But not small parts that is unique for pages or other things. For this we have helpers.

![](http://i.imgur.com/h4HmU4D.png)

A basic component looks like a mixin:

`news.jade`

```jade
mixin news(object, pagination)
	each article in object
		h3=__(News)
		+article-excerpt(article)
	if pagination
		+pagination
```

It can content any direct HTML generating code like tags or pipes of text (internationalized in my example above).

### `helpers` folder

> Helper is a part of code that you cannot use as a full component but want to store in right place.

>  Helpers ***can* be reusable**.

It can be a file with all little parts of a page that is unique for it.

For example, you have Profile page with some components like user badges and avtivity graph. But where you will store unique page header with user avatar, his score and subtitle?

It is not a component – it can but unlikely will be used somewhere else. It's small.

It's a small helper that you can store with all other helpers for that page and **quickly rearrange them in your page template**.

## Thesaurus

- **Partial** – a component, helper, mixin or any other part you can use like a solid block. *It is not a functions or other inline things.*

## Further reading:

- [Jade Guidelines](https://github.com/plentiful/guidelines/blob/master/jade.md) by Roman Zolotarev
