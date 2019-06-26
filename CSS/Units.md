### Units

### Understanding the % behavior

In addition to px and percentages, we have
root em (rem) and em (em).

In addition to that we have viewport height (vh) and
viewport width (vw).

- How is the size calculated?
- What's the right unit to choose?
- Which properties can I use in connection with
  these units?

### Where Units matter

As we know, each element is a box.

Units matter for the font size, padding, border, margin, width and height.

It also matters for the top, bottom, left and
right properties.

### How is the size calculated?

1. Absolute lengths (px)
   Mostly ignore user settings

2. Viewport lengths (vh,vw)
   Adjust our size to the current viewport

3. Font relative lengths (rem)
   Adjust to default font size

4. Percentage
   How is the box size for % units calculated?

### Three basic rules for percentages

1. Whenever an element has a position of fixed applied, the percentage is calcualted in reference to the containing block which will be the view port.

In the example below, the nav header will occupy 100% of the viewport.

```css
main-header {
	position: fixed;
	width: 100%;
}
```

2. Whenever an element has a position of absolute applied, the percentage is calculated in reference to the ancestor block's content + padding.

```css
#product-overview {
	position: relative;
}

/* Code for the tagline*/

#product-overview {
	position: absolute;
	bottom: 3%;
	left: 3%;
}
```

3. Whenever an element has a position of relative applied, the percentage is calculated in reference to the ancestor + content

### max-width

The images are mostly defined inside an image container with it's size set accordingly. The size of the image increases or decreases according the size of the container.

We could restrict our image width to a max-width even if we zoom or view the website in a larger screen.

```css
.testimonial__image-container {
	width: 65%;
	max-width: 580px;
	display: inline-block;
	vertical-align: middle;
}

.testimonial__image {
	width: 100%;
	vertical-align: top;
}
```

### Taking a look at font sizes (rem and em)

We can apply to rem to additional properties.
The rem always refers to the inital font size
of the html element.

If the default size of the font is 16px we can set our margins dynamically.

```css

.testimonial{
font-size:1.2rem;
margin:3rem 0;
}

```

### Which unit should I use?
![units](https://user-images.githubusercontent.com/15992276/59993939-0942bf80-9620-11e9-8da8-240b207cd047.JPG)

### Using auto to center elements

```css
{
margin:auto
}
```










