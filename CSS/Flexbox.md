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

