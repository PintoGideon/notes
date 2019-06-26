### Responsive Design

Which tools do we have?

### Viewport

Adjust site to the device viewport. No design changes.

# Media queries

Get an overview of the width and height of the devices
(here)[https://www.mydevice.io/]

Change design depending on the size.
Design changes defined by us.

### Understanding the viewport metatag

Apply the ratio conversion to the smaller device.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

### Media queries

```css
@media (min-width: 40rem) {
	#product-overview h1 {
		font-size: 3rem;
	}
}
```

### Finding the Right Breakpoints
