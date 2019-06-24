# The CSS Box Model

![The CSS Box model](https://user-images.githubusercontent.com/15992276/59133394-d91fcf00-8945-11e9-9003-5b11a44cabf4.JPG)

Every HTML element is interpreted as a box.

- margin is the distace between the element and it's sibbling

**_Note_**
The body also has a default margin.

```css
body {
	margin: 0;
}
```

### Margin Collapsing

![Margin](https://user-images.githubusercontent.com/15992276/59133392-d91fcf00-8945-11e9-8ea5-78356cdde79e.JPG)

If you got two elements next to each other with margins,
The margin collapse into each other and the bigger margin
wins.

### Width and Height

```css
h1 {
	width: 100%;
	height: 100%;
}
```

But the height will include the margin of the h1 element
and not span through to the entire page.

100% refers to the available height given by the parent
container. The height of the main area would only span
upto the content it holds.

**_If we want the h1 to span as long as the entire page, we need to pass the relative height of 100% to the body and html_**

```css
body {
	height: 100%;
}

html {
	height: 100%;
}
```

![The CSS Box Model 1](https://user-images.githubusercontent.com/15992276/59133393-d91fcf00-8945-11e9-8f39-40d50d1be3ac.JPG)

Setting the widhth and height does not
include the margin and the padding though.

For example:

```css
h1 {
	height: 528px;
	width: 498px;
}
```

The height displayed on the screen would be
height+ padding(both top and bottom) + border (both top and bottom)+ margin(both).

Hence we set the following property to have the above properties inclusive of the height
we want.

```css
 {
	height: 498px;
	box-sizing: border-box;
}
```

### Browser Support

Whenever we use a CSS feature, we
need to check for Browser support. Checking the browser support is important.
[Can I Use](https://caniuse.com/)

### Vertical Align

![vertically align](https://user-images.githubusercontent.com/15992276/59142143-53247800-8987-11e9-96ef-515dc69055d5.JPG)

### Inherit

The inherit CSS keyword causes the element for which it is specified to take the computed value of the property from its parent element. It can be applied to any CSS property, including the CSS shorthand all.

For inherited properties, this reinforces the default behavior, and is only needed to override another rule. For non-inherited properties, this specifies a behavior that typically makes relatively little sense and you may consider using initial instead, or unset on the all property.

Inheritance is always from the parent element in the document tree, even when the parent element is not the containing block.

**_margin: auto_**

Setting the width of a block-level element will prevent it from stretching out to the edges of its container to the left and right. Then, you can set the left and right margins to auto to horizontally center that element within its container. The element will take up the width you specify, then the remaining space will be split evenly between the two margins.

### Positioning in CSS

**_Positioning Elements_**

The elements are following a document flow.
The position property with a value of **_static_** is applied as a default to the elements.

```css
 {
	position: static;
}
```

Here are all our values:

1. absolute
2. relative
3. fixed
4. sticky

The element <div> can be positioned in terms of different contexts. For example:

1. div can be positioned 20px from the top
2. div can be positioned 20px from the viewport
3. div can be positioned 20px from the html document
4. div can be position with respect to the body

![default](https://user-images.githubusercontent.com/15992276/59557337-719cfb80-8fa5-11e9-8d5a-530f338f02a8.JPG)

![ways to change the position](https://user-images.githubusercontent.com/15992276/59557338-719cfb80-8fa5-11e9-849e-d5bc0489a230.JPG)

### fixed

An element with position: fixed; is positioned relative to the viewport, which means it always stays in the same place even if the page is scrolled. The top, right, bottom, and left properties are used to position the element.'

### absolute

An element with position: absolute; is positioned relative to the nearest positioned ancestor (instead of being positioned relative to the viewport, like fixed).

However; if an absolute positioned element has no positioned ancestors, it uses the document body, and moves along with page scrolling.

### relative

An element with position: relative; is positioned relative to its normal position.

Setting the top, right, bottom, and left properties of a relatively-positioned element will cause it to be adjusted away from its normal position. Other content will not be adjusted to fit into any gap left by the element.

### Overflow and hidden.

The element can be moved out of the parent div if it's positioned relatively.

```css
.parent .child-1{
	position:relative,
	top:300px;
	right:50px;
}
```

### sticky

Sticky is a combination of relative and fixed.

### Understanding the stacking context

```html
<div class="navigation">navigation</div>
<div class="headline">
	headline
	<div class="image-1">Image</div>
	<div class="image-2">Image</div>
	<div class="image-3">Image</div>
</div>
<div class="contact-us">Contact-us</div>
```

![Stacking Context](https://user-images.githubusercontent.com/15992276/59557512-72d02780-8fa9-11e9-8d39-cf240b6a6fd4.JPG)

### Working with Modals

```html
<div class="backdrop"></div>
<div class="modal">
	<h1 class="modal__title">Do you want to continue?</h1>
	<div class="modal__actions">
		<a href="start-hosting/index.html" class="modal__action">Yes</a>
		<button class="modal__action modal__action--negative" type="button">
			No
		</button>
	</div>
</div>
```

```css
.modal {
	position: fixed;
	z-index: 200;
	display: none;
	top: 20%;
	left: 30%;
	width: 40%;
	background: white;
	padding: 1rem;
	border: 1px solid #ccc;
}
```

