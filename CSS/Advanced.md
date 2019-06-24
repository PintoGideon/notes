### Understanding Background size

The **_cover_** property makes sure that
the image always fills the container.

It makes the necessary adjustment
to the width or height to fit the container.

```css
#product-overview {
	background-image: url('');
	background-size: cover;
	background-position: 0% 10%;
	width: 100%;
	height: 528px;
	padding: 10px;
	margin-top: 43px;
	position: relative;
}
```

The background position property decided which portion of the image needs to be cropped from the left,right, top or bottom.

```css
background-position: left top;
```

This tells us that no portion of the image from the left and top should be cropped.

### Linear Gradients

Gradients are treated as images.

```css
background-image: linear-gradient(to bottom, red, blue);
```
The color will transition from top to bottom.

***The radial gradient ***

```css
/*The shape is an ellipse*/
background-image:radial-gradient(red,blue)

```


### Filters

- [Filters](https://developer.mozilla.org/en-US/docs/Web/CSS/filter)

### Adding and Styling SVG's

We can add svg's in their own container as an inline block.

```html
<svg></svg>
```








