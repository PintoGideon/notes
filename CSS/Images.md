### Styling Images

**_Styling a logo_**

```html
<div>
	<a href="index.html" class="main-header__brand">
		<img src="images/logo.jpg" alt="testing logo" />
	</a>
</div>
```

```css
.main-header__brand {
	text-decoration: none;
	font-weight: bold;
	font-size: 22px;
	height: 22px;
	display: inline-block;
}

.main-header__brand img {
	height: 100%;
}
```

### Working with image layouts (just the key parts without context)

```html
<div class="testimonial__image-container">
	<img
		src="../images/customer-1.jpg"
		alt="Mike Statham-Customer"
		class="testimonial__image"
	/>
</div>
```

```css
.testimonial__image-container {
	width: 80%;
	display: inline-block;
	box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.3);
}

.testimonial__image {
	width: 100%;
}
```
