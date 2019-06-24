```html

<header class="main-header">

 <div><a class="main-header__brand">Brand Logo</a></div>
  <nav>
   <ul>
    <li><a>Home</a></li>
    <li><a>About</a></li>
    <li><a>Contact me</a></li>
   <ul>
  </nav>
</header>

```

- Add a background Color
- Add a padding
- Set the width the full page width

```css
.main-header {
	width: 100;
	background: #2ddf5c;
	padding: 8px 16px;
}

.main-header > div {
	display: inline-block;
	vertical-align: middle;
}
.main-nav {
	display: inline-block;
	width: calc(100% - 49px);
}
```

### Display Property

In HTML, we have block level and inline elements.
The block level element takes the full width of the page
while the inline element takes the width the content
needs.

```css
ul {
	margin: 0;
	padding: 0;
	list-style: none;
}
li {
	display: inline-block;
	margin: 0 16px;
}

li a {
	text-decoration: none;
	color: #2ddf5c;
	font-weight: bold;
	border-bottom: 5px solid white;
	padding: 3px 0;
}
```

### Pseudo Classes and Pseudo Elements

**_An Example of A Pseduo Element_**

```html
<p>Freedom in Christ My Savior</p>
```

```css
p::first-letter {
	color: red;
	font-size: 20px;
}
```

---

**\*Using Pseudo classes in our code\*\***

```css
li a:hover {
	text-decoration: none;
	color: white;
}

li a:after {
	content: '(Link)';
	color: red;
}

li a:active {
	color: white;
}
```

![nav](https://user-images.githubusercontent.com/15992276/59135859-93b3cf80-894e-11e9-91d4-dce564a59aee.JPG)

Here the li will behave as an inline element but also give you
the ability to set margin and padding to it.

### Styling the logo

```css
.main-nav__brand {
	color: #0e4f1f;
	text-decoration: none;
	font-weight: bold;
	vertical-align: middle;
}
```

### More CSS

```html
<main>
	<section id="product-overview">
		<h1>Christ My Savior</h1>
	</section>

	<section id="plans">
		<h1 class="section-title">Choose Your Plan</h1>
		<div>
			<article class="plan">
				<h1 class="plan__annotation">Free</h1>
				<h2 class="plan__title">$0/month</h2>
				<h3>For Hobby projects or small teams.</h3>

				<ul class="plan__features">
					<li>1 workspace</li>
					<li>Unlimited Traffic</li>
				</ul>

				<div>
					<button>Choose Plan</button>
				</div>
			</article>

			<article class="plan plan--highlighted">
				<h1 class="plan__annotation">Recommended</h1>
				<h2>$0/month</h2>
				<h3>For Hobby projects or small teams.</h3>

				<ul class="plan__features">
					<li>1 workspace</li>
					<li>Unlimited Traffic</li>
				</ul>

				<div>
					<button>Choose Plan</button>
				</div>
			</article>
		</div>
	</section>
</main>
```

```css
.plan {
	background: #d5ffdc;
	text-align: center;
	padding: 16px;
	margin: 8px;
	display: inline-block;
	width: 30%;
	vertical-align: middle;
}

.plan--highlighted {
	background: #19b84c;
	color: #fff;
	box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.5);
}

.plan__annotation {
	background: white;
	color: #19b84c;
	padding: 8px;
	box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.5);
	border-radius: 8px;
}

.plan__title {
	color: #0e4f1f;
}

.plan__features {
	list-style: none;
	margin: 0;
	padding: 0;
}

.plan__features li {
	margin: 8px 0;
}

/*Button inherited styles set by the browser */

.button {
	background: #0e4f1f;
	color: white;
	font: inherit;
	border: 1.5px solid #0e4f1f;
	padding: 8px;
	border-radius: 8px;
	font-weight: bold;

	/* Adding an icon whenever you hover over a button */

	cursor: pointer;
}

.button:hover,
.button:active {
	background: white;
	color: green;
}
```

### Styling a hamurger menu

```html
<div>
	<button class="toggle-button">
		<span class="toggle-button__bar"></span>
		<span class="toggle-button__bar"></span>
		<span class="toggle-button__bar"></span>
	</button>
</div>
```

```css
.toggle-button {
	width: 3rem;
	background: transparent;
	border: none;
	cursor: pointer;
	padding-top: 0;
	padding-bottom: 0;
	vertical-align: middle;
}

.toggle-button__bar {
	width: 100%;
	height: 0.2rem;
	background: black;
	margin: 0.6rem 0;
	display: block;
}
```
