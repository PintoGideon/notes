### Grids

/_ Declare a grid layout, set up 3 columns and a grid gap _/

```css
.wrapper {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
	grid-gap: 1em;
}
```

/_ How many columns will each class span? Here we've used numbers to call out specific locations of some boxes _/

```css
.a {
	grid-column: 1/3;
}

.h {
	grid-column: 2/4;
}
```

We can also define our layouts using grid-template-areas.

```css
grid-template-areas:
"a a b"
"c c c"
"d e f"
"g h h";
grid-gap:1en;

.a {
	grid-area: a;
}
.b {
	grid-area: h;
}
.c {
	grid-area: c;
}
```


