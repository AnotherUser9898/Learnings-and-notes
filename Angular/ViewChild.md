# View Child and getting access to template elements

We can access elements in the template of an angular component by using the `@ViewChild()` decorator or the `viewChild()` function.

## Syntax

### `@ViewChild`

```typescript
@ViewChild(selector) variable: ElementRef<typeof SelectedElement>;
```

> The selector parameter does not correspond to a CSS selector! and is
> completely different.

The `@ViewChild()` decorator takes a selector parameter which is used to select the element in the component template.

The selector maybe the class name of a component, in which case the instance of that class if present in the component template will be selected, the selector can also be a string value corresponding to a template variable defined in the component template which would select the element which has that template variable defined for it.

The selected element is then wrapped in an `ElementRef` Object and stored in the provided variable.

The type annotation for the variable is written accordingly, as seen in the above code snippet the type of the variable is `ElementRef<typeof SelectedElement> | undefined`, `undefined` is added because there is no guarantee of finding the element in the template.

### `viewChild` Signal Function

The ### `viewChild` function syntax is very similar to the above except that this function returns a `signal` value.

```typescript
variable = viewChild<ElementRef<typeof SelectedElement>>(selector);
```

The type of the variable in the above code snippet is `ElementRef<typeof SelectedElement> | undefined` for similar reasons to the decorator based approach.

However, the signal based approach provides a way to get rid of the `undefined` in the return type;

```typescript
variable = viewChild.required<ElementRef<typeof SelectedElement>>(selector);
```

By adding `required` angular throws an error if such an element is not found and this convinces TypeScript that the element will always be found.

## Usage

We can now gain access to the selected template element in the following way:

```typescript
this.variable.nativeElement; // Decorator

this.variable.nativeElement(); // Signal
```

## Aside - Storing multiple template elements using `@ViewChildren`

You can store multiple template elements corresponding to the same selector as an array of `ElementRefs` using `@ViewChildren`.
