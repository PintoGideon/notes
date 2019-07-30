### Property

We can add flexbox to the display property. When we add
flex to the display property, we turn the element
into a Flex Container.

The elements inside the flex container are children and
are called flex items.

Here the following properies we can apply:

- flex-flow
- justify-content
- align-content
- align-items

### Creating a flex container

```html
<div class="flex-container">
	<div class="item-1"></div>
	<div class="item-2"></div>
</div>
```

```css
.flex-container {
	display: flex;
	flex-direction: row;
	flex-wrap: wrap;
	/*flex-row:row rap;*/
}
```

### Understanding Main Axis vs Cross Axis

![default axis flow](https://user-images.githubusercontent.com/15992276/60152533-d549d480-97ae-11e9-958e-65748b6bbf0b.JPG)

### Working with align items

With the align items property, it center's the items
on the cross axis.
![Aligning items](https://user-images.githubusercontent.com/15992276/60152532-d549d480-97ae-11e9-82ec-08818396d7ae.JPG)

```css
align-items: center;
justify-content: center;
```

With justify-content, the items are centered across the main axis.

**_And What about Align Content?_**
Aligns the elements on the cross axis.

```css
align-content: center;
```

### Order and Flex Shorthand

The order property can have different values. It's default is 0.

If we provide a value of 1 to the order property, the element will appear at the end of the list of items. A value of -1 will place it at the start of the list of items.

### align self

If you want to position an element in relation to
the cross axis.

```css
 {
	align-self: start;
}
```

### flex-grow

Flex grow has an initial value of zero. The flex grow property allows the element to use the space avaiable
when the screen size increases.

Now both the item have a total of 3 parts. The space available will be distributed between these two items.
1/3 is applied to item-1 and 2/3 is applied to item-2 of the total space available.

```css
.item-1 {
	flex-grow: 1;
}

.item-2 {
	flex-grow: 2;
}
```

### flex-shrink

If we add the flex shrink property with a value of 0 to an element, the width of the element will not decrease below the set width even if the screen increases/decreases.

### flex-basis

The flex-basis property is a sub-property of the Flexible Box Layout module.

It specifies the initial size of the flex item, before any available space is distributed according to the flex factors. When omitted from the flex shorthand, its specified value is the length zero.

A flex-basis value set to auto sizes the element according to its size property (which can itself be the keyword auto, which sizes the element based on its contents

```css
.item-5 {
	flex-basis: 300px;
}
```

```css

flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]

/* this */
  flex: 1 100px;

  /* is the same as */
  flex-grow: 1;
  flex-basis: 100px;
```

See a practical example [here](https://codepen.io/HugoGiraudel/pen/fec8936890d14e842ac4856ce34e5fbe)
