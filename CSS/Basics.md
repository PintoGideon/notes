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
h1{
height:528px;
width:498px
}
```

The height displayed on the screen would be
height+ padding(both top and bottom) + border (both top and bottom)+ margin(both).

Hence we set the following property to have the above properties inclusive of the height
we want.

```css
{
    height:498px;
    box-sizing:border-box;
}
```












