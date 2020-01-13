### Spreading into object literals (...)

```javascript
const obj = {
  foo: 1,
  bar: 2
};
```

If property keyes clash, the property that is mentioned last "wins".

```javascript
const obj = {
  foo: 1,
  bar: 2,
  baz: 3
};

{...obj, foo:true}

{foo:true, bar:2, baz:3}

```



