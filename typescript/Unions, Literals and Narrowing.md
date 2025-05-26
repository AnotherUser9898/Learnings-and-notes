#typescript
>References: 
> - [Stack Overflow answer explaining the types](https://stackoverflow.com/questions/38855908/naming-of-typescript-union-and-intersection-types)
> - [Total Typescript - Discriminated Unions](https://www.totaltypescript.com/books/total-typescript-essentials/unions-literals-and-narrowing#discriminated-unions)

# Discriminated Unions

## The Problem: Bag of Options

Let's imagine we are modelling a data fetch we have a `state` type with a `status`  property which can be in one of three states: `error`, `success` and `loading`.

```typescript
type state = {
	status: 'loading' | 'error' | 'success';
}
```

This is useful, but we also need to capture data about the result of the fetch, which we could do like this by adding optional error and data properties. 

```typescript
type state = {
	status: 'loading' | 'error' | 'success';
	error?: string;
	data?: string

}
```

Let's also imagine that there is a front end function that renders a string based on the status of the fetch.

```typescript
function renderUI(state: state) {
	if (state.status == 'loading') {
		return '...loading';
	}
	else if (state.status == 'error') {
		return `Error: ${state.error}` // Error: 'state.error' is possibly 'undefined'.
	}
	else {
		return `Data: ${state.data}`;
	}
	
}
```

This all looks good except for the error we get when accessing the `data` property on the `state` object.

This is because we have defined our state type incorrectly, our definition does associate the error status and the `error` property or the success status and the `data` property. Typescript doesn't know of these associations and hence its inference does not work very well in this scenario.

## The Solution: Discriminated Union

The solution is to turn our type into a **discriminated union**.

A discriminated union is a union type, in which each member of the union has a common property, the 'discriminant' which is a literal type unique to each member of the union.

In our case, the `status` property is the discriminant.

Let's take each status and make them into separate object literals.

```typescript
type state = {
	{
		status: 'loading'
	} 
	|
	{
		status: 'error';
		error: string
	}
	|
	{
		status: 'success';
		data: string
	}
};
```

Now that we have associated the `status` and their respective `error` and `data`  properties, typescript's type narrowing and inference works correctly now, identifying that `error` property is present in the `state` object when the `status` is error.

We could even clean up the code using type aliases to make it a bit more organised.

```typescript
type LoadingState = {
  status: 'loading'
};

type ErrorState = {
  status: 'error'
  error: string
};

type SuccessState = {
  status: 'success'
  data: string
};

type State = LoadingState | ErrorState | SuccessState;
```
