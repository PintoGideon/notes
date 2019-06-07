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
    text-decoration:none;
	color: white;
}

li a:after{
    content: "(Link)";
    color:red;
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
