### Interfaces

```typescript
type Pizza = {
  name: string;
  sizes: string[];
};

let pizza: Pizza;

function createPizza(name: string, sizes: string[]) {
  return {
    name,
    sizes,
  };
}

pizza = createPizza("Pepperoni", ["small", "medium"]);
```

Interface is just a preferred approach while dealing with complex datasets. It is a contractual agreeement between a variable and data structure.

```javascript
interface Pizza {
  name: string;
  sizes: string[];
  getAvailableSizes(): string[];
}

interface Pizzas {
  data: Pizza[];
}
```

```javascript
function myFunction() {
  console.log(this);
}

myFunction(); // Window
```

### Typing 'this' and 'noImplicitThis'

```html
<div>
  <a href="" class="click">Click here</a>
</div>
```

```javascript
const element = document.querySelector(".click");
function handleClick(this: HTMLAnchorElement, event"Event) {
  event.preventDefault();
  console.log(this.href);
}

elem.addEventListener("click", handleClick, false);
```

### Using a typeof operation in Typescript

```typescript
const person = {
  name: "Todd",
  age: 17,
};

type Person = typeof person;

const person2: Person = {
  name: "John",
  age: "30",
};
```

### keyof operator

```typescript
type PersonKeys = keyof Person; // "name" | "age"

type PersonTypes = Person[PersonKeys]; //"string" | "number"
```

