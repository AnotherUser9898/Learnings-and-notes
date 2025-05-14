#Typescript-only-features 

> Reference: [Total Typescript- Class Parameter Properties](https://www.totaltypescript.com/books/total-typescript-essentials/typescript-only-features#class-parameter-properties)

One TypeScript feature that does not exist in JavaScript is Class Parameter Properties. These allow you to declare and initialize class members directly from the constructor of the [[Class]].

Consider this Rating class: 

```typescript
class Rating {
	constructor (public value: number,private max: number) {}
}
```

This code includes `public` before the `value` parameter and `private` before the `max` parameter. This is more than anything just a shortcut to declaring and initializing class properties in one step.

The compiled JavaScript code assigns properties to the class in the following way

```javascript
class Rating {
	constructor (value,max) {
		this.value = value;
		this.max = max;
	}
}
```

