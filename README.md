# C2 CSS Style Guide

### Table of contents

1. [Why](#why)
2. [How](#how)
3. [Example 1](#example-1)
4. [Further reading](#further-reading)

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

In [Mackillop](http://mackillop.c2gloo.net/) we have a simple sub-pages section.

![Mackillop sub-pages example](https://github.com/lclghst/css/blob/master/img/example1.png)

Quite simple when we break it down:

A subpage consists of image on left, description on right.

Here is the css selector for the description `body.landing-page div.sub-pages>div.row` `div.description` AND `body.landing-page div.left div.description`.

The html structure looks like
```html
<div class=”subpages”>
	<div class=”row”>
		<div class=”thumb”><a><img /></a></div>
		<div class=”description”>
			<h3/>
			<p/>
			<a class=”read-more” />
		</div>
	</div>
</div>
```
And our css selectors look like
```css
~~body~~.landing-page div.sub-pages{}
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
1. We do not want any references to HTML in our css for specicifity reasons (I'll go into more detail later)
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
## Further reading

* https://en.bem.info/
* http://getbem.com/
* http://www.smashingmagazine.com/2012/04/a-new-front-end-methodology-bem/
* https://css-tricks.com/bem-101/
* http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/
