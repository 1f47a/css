# C2 CSS Style Guide

![CSS](http://i.imgur.com/Q3cUg29.gif)

### Table of contents

1. [Why](#why)
2. [How](#how)
3. [Example 1](#example-1)
4. [Benefits](#benefits)
5. [Modifiers](#modifiers)
6. [Javascript](#javascript)
7. [Animation](#animation)
8. [tl;dr](#tldr)
9. [The Future](#the-future)
10. [Further reading](#further-reading)

## Why

We need to change our approach to CSS. 

This isn't a ruleset that you must follow 100% and salute before you go to bed but rather a guide to how we can make our CSS:

* Clearer code
* Easier maintenance
* Write less code
* Separation of markup and style
* Faster development
* Faster loading pages

## How

Using a method called [BEM](https://en.bem.info/).

BEM stands for Block Element Modifier and it is more simple than it sounds.

## Example 1

I will pick on myself here with some examples of CSS I have written for previous projects.

In [Mackillop](https://www.mackillop.org.au/) we have a simple sub-pages section.

![Mackillop sub-pages example](https://github.com/lclghst/css/blob/master/img/example1.png)

Quite simple when we break it down:

A subpage consists of image on left, description on right.

Here is the css selector for the description `body.landing-page div.sub-pages>div.row` `div.description` AND `body.landing-page div.left div.description`.

The HTML structure looks like

```html
<div class="subpages">
	<div class="row">
		<div class="thumb"><a><img /></a></div>
		<div class="description">
			<h3/>
			<p/>
			<a class="read-more" />
		</div>
	</div>
</div>
```

And our css selectors look like

```css
body.landing-page div.sub-pages{}
body.landing-page div.sub-pages > div.row{}
body.landing-page div.sub-pages > div.row h3{}
body.landing-page div.sub-pages > div.row h3:hover{}
body.landing-page div.sub-pages > div.row p{}
body.landing-page div.sub-pages > div.row div.description{}
.read-more{}
.read-more:hover{}
body.landing-page div.sub-pages .thumb{}
body.landing-page div.left div.description p{}
body.landing-page div.left div.description{}
body.landing-page .thumb{}
```

So how can we apply our BEM guidelines to this?

1. We do not want any references to HTML in our css for specificity reasons (I'll go into more detail later)

   eg. ~~body~~`.landing-page `~~div~~`.sub-pages>`~~div~~`.row `~~div~~`.description`

2. The direct child selector `>` is probably overkill in 99% of cases. Why not just omit it here, along with `div.row`?

  This way we are keeping our CSS separate from Bootstrap

   eg. ~~body~~`.landing-page `~~div~~`.sub-pages`~~> div.row div~~`.description`

3. What if we want this subpage module on a different template page? At the moment it only works on the landing page.

   eg. ~~body .landing-page div~~`.sub-pages`~~> div.row div~~`.description`

4. Taking this one step further, we could put the description into its own class.

   eg. `.sub-pages__description`

This technique is taken from [BEM](https://en.bem.info/).

Let's apply it to our sub-pages module, so our HTML looks more like

```html
<div>
	<div class=”subpage”>
		<div class=”subpage__thumb”><a><img /></a></div>
		<div>
			<h3 class=”subpage__title”/>
			<p class=”subpage__description”/>
			<a class=”read-more” />
		</div>
	</div>
</div>
```

And our CSS selectors look like

```css
.subpage{}
  .subpage__thumb{}
  .subpage__description{}
  .subpage__title{}
    .subpage__title:hover{}
.read-more{}
```

Wow, that's a lot less code already. Now if someone was to go back and make any style updates, it is a lot easier for them to understand exactly what will be affected by their changes.

Also we can now safely put the subpages module on any other page we like, without having to rewrite our code.

To further this example, now that we have uncoupled the style from the markup, we can even change how the modules HTML is structured and not worry about having to change our selectors.

   eg. We have to change the subpages into list elements

```html
<ul>
	<li class=”subpage”>
		<div class=”subpage__thumb”><a><img /></a></div>
		<div>
			<h3 class=”subpage__title”/>
			<p class=”subpage__description”/>
			<a class=”read-more” />
		</div>
	</li>
</ul>
```

## Modifiers

Blah

## Benefits

From our example above, our CSS has become:

##### Modular

   We can put the subpage module anywhere else in the website and not have to write any more CSS for it.
   
##### Flexible

   If we have to make changes to the HTML structure, we can just add whatever we need to, without changing the selector or class name.
   
##### Faster

   Believe it or not, our CSS and web pages will load faster now. Not only because we have reduced the length of the class names but because we have reduced how specific our selectors are.
   
   CSS is a funny beast and it actually reads a CSS selector from right to left. Crazy huh?
   
   Take for example `body.landing-page div.sub-pages > div.row div.description{}`
   First it looks for all elements with the `.description` class, then only the `<div>` with `.description`, then only the `<div>` with `.description` that are inside a `.row` parent, then only the `<div>` with `.description` that are inside a `.row` parent that is a `<div>` etc etc

   So you can see it is a lot more efficient (and quicker) for our browsers to only have to match the single class.

## Javascript

![Chris at work](http://i.imgur.com/rZDKwoj.gif)

Now we have uncoupled our CSS from our HTML, it is time to do the same with our Javascript.

There is no problem binding a Javascript event to a css class, it only becomes a problem when we use a class that is being used for style.

The problem being that at the moment you can accidentally break our Javascript by changing a class name, or HTML element.

What we can do instead is create a new class name that has no CSS styles associated with it, preferably prefixed with `js` so we can be even more clear in what the class is for.

For example

```html
<button class=”subscribe__submit btn”>Submit</button>
```

and our js
```javascript
$('.subscribe_submit').click(function ()
{
	$(this).parent().find('ul').slideToggle('fast', function () { return false; });
});
```

We apply our new `js` only class `js-toggleParent`.

```html
<button class=”subscribe__submit btn js-toggleParent”>Submit</button>
```

Update our Javascript code

```javascript
$('.js-toggleParent').click(function ()
{
	$(this).parent().find('ul').slideToggle('fast', function () { return false; });
});
```

Now we can change styles with ease. Also it is quite clear that there is a Javascript event attached to this element and the name tells us what it does.

Yes it is long, yes it is ugly but it makes it a whole lot easier for someone to look at your code and know what is going on and for someone else to make any changes without breaking the styles or scripts.

## Animation

A few performance gains we can easily make.

Try not to use jQuery `$.animate()`. 

Instead of moving elements with `Top` `Right` `Bottom` `Left` use `translate` and `transform`.
This applies also to using `position: absolute`. Positioning with `transform` avoids choppy FPS and poor animation performance.

## tl;dr

![didn't read lol](http://i.imgur.com/Na81R.gif)

This guide is to help us all write better CSS. But it is just that - a guide.

By putting a little more effort into our class names and separating our CSS from our HTML and Javascript, we can make developing and maintaining our front-end code a lot nicer.

## The future

* blah

## Further reading

* https://en.bem.info/
* http://getbem.com/
* http://www.smashingmagazine.com/2012/04/a-new-front-end-methodology-bem/
* https://css-tricks.com/bem-101/
* http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/
