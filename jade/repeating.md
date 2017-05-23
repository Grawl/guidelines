# Repeating in Pug

You can do like this:

```pug
-
	const list = [
		{
			title: 'Title 1',
			description: 'Description 1',
		},
		{
			title: 'Title 2',
			description: 'Description 2',
		},
		{
			title: 'Title 3',
			description: 'Description 3',
		},
	]
each item in list
	.list-item
		h3=item.title
		p=item.description
```

```html
<div class="list-item">
	<h3>Title 1</h3>
	<p>Description 1</p>
</div>
<div class="list-item">
	<h3>Title 2</h3>
	<p>Description 2</p>
</div>
<div class="list-item">
	<h3>Title 3</h3>
	<p>Description 3</p>
</div>
```

If you want to vary some item, you have to add another key-value pair into list object:

```pug
-
	const list = [
		{
			title: 'Title 1',
			description: 'Description 1',
		},
		{
			title: 'Title 2',
			description: 'Description 2',
			selected: true,
		},
		{
			title: 'Title 3',
			description: 'Description 3',
		},
	]
each item in list
	.list-item(class=item.selected&&'checked')
		h3=item.title
		p=item.description
```

```html
<div class="list-item">
	<h3>Title 1</h3>
	<p>Description 1</p>
</div>
<div class="list-item checked">
	<h3>Title 2</h3>
	<p>Description 2</p>
</div>
<div class="list-item">
	<h3>Title 3</h3>
	<p>Description 3</p>
</div>
```

Imagine you want to wrap an item into other element instead of add a class.

Then, use mixins instead:

```pug
mixin item(options)
	.list-item
		h3=options.title
		p=options.description
+item({
	title: 'Title 1',
	description: 'Description 1',
})
.wrapper: +item({
	title: 'Title 2',
	description: 'Description 2',
})
+item({
	title: 'Title 3',
	description: 'Description 3',
})
```

```html
<div class="list-item">
	<h3>Title 1</h3>
	<p>Description 1</p>
</div>
<div class="wrapper">
	<div class="list-item checked">
		<h3>Title 2</h3>
		<p>Description 2</p>
	</div>
</div>
<div class="list-item">
	<h3>Title 3</h3>
	<p>Description 3</p>
</div>
```

**Bonus: attributes injecting!**

You can vary attributes inside one (just one, sorry) of any elements inside mixin using `&attributes` trick:

```pug
mixin item(options)
	li.list-item
		h3&attributes(attributes)=options.title
		p=options.description
+item({
	title: 'Title 1',
	description: 'Description 1',
})(style='color: red')
```

So, in HTML it will be following:

```html
<li class="list-item">
	<h3 style="color: red">Title 1</h3>
	<p>Description 1</p>
</li>
```
