#Typescript-only-features 

>Reference: [Total TypeScript - Enums](https://www.totaltypescript.com/books/total-typescript-essentials/typescript-only-features#enums)

Enums are a TypeScript exclusive feature that define a set of names constants. These can be used as types or values.

A good use case for Enums is when you need to store a set of related values that will aren't expected to change.

You can declare Enums using the `enum` keyword.

## Numeric Enums

A Numeric Enum groups together a set of related values and automatically assigns each of them a numeric value starting from 0.

Consider the following `AlbumStatus` Enum: 

```typescript
enum AlbumStatus {
  NewRelease, // AlbumStatus.NewRelease = 0
  OnSale, // AlbumStatus.OnSale = 1
  StaffPick,// AlbumStatus.StaffPick = 2
}
```

In the above code snippet the value of `AlbumStatus.NewRelease` is 0, value of `AlbumStatus.OnSale` is 1 and the value of `AlbumStatus.StaffPick` is 2.

We can also use the `AlbumStatus` Enum as a type like so: 

```typescript
function logStatus(genre: AlbumStatus) {
	console.log(genre);
}
```

Now `genre` can only accept values from the `AlbumStatus` Enum object.

```typescript
logStatus(AlbumStatus.NewRelease);
```


### Numeric Enums with Explicit Values

You can also assign explicit values to Enum members.

```typescript
enum AlbumStatus {
  NewRelease = 1,
  OnSale = 2,
  StaffPick = 3,
}
```


### Auto-incrementing Numeric Enums

If you only assign explicit values to some members then typescript assigns values to the other members by automatically incrementing the last explicitly assigned value. For example if we only assign a value to `NewRelease`, say 1, then the values of `OnSale` and `StaffPick` will be 2 and 3.

```typescript
enum AlbumStatus {
  NewRelease = 1,
  OnSale, // 2
  StaffPick, // 3
}
```


## String Enums

String Enums allow you to assign a string value to each member of the Enum.

```typescript
enum AlbumStatus {
  NewRelease = "NEW_RELEASE",
  OnSale = "ON_SALE",
  StaffPick = "STAFF_PICK",
}
```


## Enums are Strange

Enums exhibit odd behavior in certain circumstances.

### How Numeric Enums Transpile

The way Numeric Enums are converted to JavaScript code can feel slightly unexpected.

For Example the Enum `AlbumStatus` : 
```typescript
enum AlbumStatus {
  NewRelease,
  OnSale,
  StaffPick,
}
```

transplies to the following JavaScript: 
``` javascript
var AlbumStatus;
(function (AlbumStatus) {
  AlbumStatus[(AlbumStatus["NewRelease"] = 0)] = "NewRelease";
  AlbumStatus[(AlbumStatus["OnSale"] = 1)] = "OnSale";
  AlbumStatus[(AlbumStatus["StaffPick"] = 2)] = "StaffPick";
})(AlbumStatus || (AlbumStatus = {}));
```

This rather complex looking piece of code does several things in one go.
- It creates an empty object `AlbumStatus`.
- Creates keys corresponding to each of the Enum members on the `AlbumStatus` object and assigns the value of each of the Enum members to their respective keys in `AlbumStatus` object.
- It then also creates a reverse mapping between the same keys and values within the `AlbumStatus` object.

The final object looks something like this:
```javascript
AlbumStatus = {
	NewRelease: 0,
	OnSale: 1,
	StaffPick: 2,
	0: "NewRelease",
	1: "OnSale",
	2: "StaffPick"
}
```
This reverse mapping means that there are more keys on the object than you expect which can become a source of confusion if you are not aware of this.


### How String Enums Transpile

The behavior of string Enums is much more predictable.

For a string Enum like this:

```typescript
enum AlbumStatus {
  NewRelease = "NEW_RELEASE",
  OnSale = "ON_SALE",
  StaffPick = "STAFF_PICK",
}
```

The transpiled JavaScript code is this:

```javascript
var AlbumStatus;
(function (AlbumStatus) {
  AlbumStatus["NewRelease"] = "NEW_RELEASE";
  AlbumStatus["OnSale"] = "ON_SALE";
  AlbumStatus["StaffPick"] = "STAFF_PICK";
})(AlbumStatus || (AlbumStatus = {}));
```

Objects generated from string Enums contain no reverse mappings.

The differences between [[#Numeric Enums]] and [[#String Enums]] feels inconsistent and can be source of confusion.

### Numeric Enums behave like Union Types

Another odd feature is that string Enums and Numeric Enums behave differently when used as types.

Consider the following code snippet:
```typescript
enum AlbumStatus {
  NewRelease = 0,
  OnSale = 1,
  StaffPick = 2,
}

function logStatus(genre: AlbumStatus) {
  console.log(genre);
}
```

Now we could call the `logStatus` function with any member from `AlbumStatus` like so:
```typescript
logStatus(AlbumStatus.NewRelease);
```

This is expected behavior, what becomes a source of confusion however is when we use the value of the Enum member itself to call the function.

```typescript
logStatus(0);
```

The above code does not throw an error and compiles successfully.

Do note that if we call the `logStatus` function with a number that does not correspond to any Enum member then TypeScript throws an Error.

```typescript
logStatus(3); // Error: Argument of type '3' is not assignable to parameter of type 'AlbumStatus'.
```

This is different from [[#String Enums]] which only allow Enum members to be used as types:

```typescript
enum AlbumStatus {
  NewRelease = "NEW_RELEASE",
  OnSale = "ON_SALE",
  StaffPick = "STAFF_PICK",
}

function logStatus(genre: AlbumStatus) {
  console.log(genre);
}

logStatus(AlbumStatus.NewRelease);
logStatus("NEW_RELEASE"); // Error: Argument of type '"NEW_RELEASE"' is not assignable to parameter of type 'AlbumStatus'.
```

Another difference is that [[#String Enums]] are compared nominally (based on their name) instead of structurally like everything else in TypeScript, this means that two [[#String Enums]] with the same structure but different names will be treated as different.

### const Enums

Const Enums are like normal Enums except for the fact that they are declared with a `const` keyword:

```typescript
const enum AlbumStatus {
  NewRelease = "NEW_RELEASE",
  OnSale = "ON_SALE",
  StaffPick = "STAFF_PICK",
}
```

`const` Enums have the same behavior as regular Enums. The major difference is that `const` Enums are completely transpiled away when TypeScript is transpiled into JavaScript, the transpiled JavaScript code uses the values from the const Enum directly.

```typescript
let albumStatuses = [
  AlbumStatus.NewRelease,
  AlbumStatus.OnSale,
  AlbumStatus.StaffPick,
];

// the above transpiles to:
let albumStatuses = ["NEW_RELEASE", "ON_SALE", "STAFF_PICK"];
```

> The TypeScript team actually recommends avoiding `const` Enums in your library code because they can behave unpredictably for consumers of your library.

