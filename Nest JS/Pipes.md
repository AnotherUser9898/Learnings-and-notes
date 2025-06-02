#pipes #nestjs 

A Pipe in Nest JS is a class annotated with an `@Injectable` decorator, and one which implements the `PipeTransform` interface.

Pipes in Nest JS typically have two use cases: -
- Transformation - Transform input data to the desired form. eg (from string to integer)
- Validation - Evaluate input data and if valid, simply pass it through unchanged, otherwise throw and exception.
In both cases, pipes operate on data destined for a controller route handler. Nest interposes a pipe just before the controller method receives them. The pipes then process the data, the processed and potentially transformed data is then passed to the request handler method using which the request handler method is invoked.

Nest comes with a lot of built-in pipes, we can also build custom pipes.

> Pipes run inside the exceptions zone. This means that when a Pipe throws an exception it is handled by the exceptions layer (global exceptions filter and any [[Exception Filters]] that are applied to the current context). Given the above, it should be clear that when an exception is thrown in a Pipe, no controller method is subsequently executed. This gives you a best-practice technique for validating data coming into the application from external sources at the system boundary.

## Binding Pipes

To use a pipe, we need to bind an instance of the pipe class to the appropriate context. In this example we will use the `ParseIntPipe` and associate it with a particular route handler method. 

```typescript
@Get()
async getCat(@Param('id', ParseIntPipe) id: number) {
	return this.catsService.findOne(id);
}
```

This ensures that either the call to `catService` gets a number as expected or we throw an exception message like this: 

```json
{
  "statusCode": 400,
  "message": "Validation failed (numeric string is expected)",
  "error": "Bad Request"
}
```

The exception will prevent the body of `findOne` from executing.

In the example above we pass a class not an instance, leaving the job of creating an instance to the framework, we can also pass an in place instance if we so wish. Passing an in place instance is useful if we want to provide options to the pipe to customise its built in behaviour.

```typescript
@Get()
async getCat(
	@Param('id', new ParseIntPipe({errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE}))) id: number
	) {
	return this.catsService.findOne(id);
}
```

Binding all other pipes works similarly.

## Custom Pipes

Nest gives us the ability to create custom pipes. A custom pipe is class annotated with an `@Injectable` decorator that implements the `PipeTransform` interface.

```typescript
@Injectable()
class CustomPipe implements PipeTransform {
	transform(value: any, metadata: ArgumentMetadata) {
		console.log(value);
		return value;
	}
}
```

The `PipeTransform` interface requires the class to implement a `transform` method, this method contains two parameters, namely value of type any and metadata of type `ArgumentMetadata` . 

- value - The value is the currently processed method argument.

```typescript
@Get()
async getCat(@Param('id', CustomPipe) id: number) {
	return this.catsService.findOne(id);
}
```

In this example the `id` parameter is the value passed to the `value` parameter of the pipe before going to the route handler.

- metadata - This is the currently processed method arguments metadata.

The `metadata` parameter has the following properties: 

```typescript 
export interface ArgumentMetadata {
  type: 'body' | 'query' | 'param' | 'custom';
  metatype?: Type<unknown>;
  data?: string;
}
```

- type: Indicates whether the argument is a body `@Body()`, query `@Query()`, param `@Param()`, or a custom parameter.
- metatype: Provides the metatype of the argument, for example, `String`. Note: the value is `undefined` if you either omit a type declaration in the route handler method signature, or use vanilla JavaScript. In the above example with `CustomPipe` the metatype is the `string` type which translates to the `String` constructor function being held in the metatype property.
- data: The string passed to the decorator for example string in `@Body('string')`.  It's `undefined` if you leave the decorator parenthesis empty.
## Global scoped pipes

Since the `ValidationPipe` was created to be as generic as possible, we can realize its full utility by setting it up as a **global-scoped** pipe so that it is applied to every route handler across the entire application.

```typescript

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```

Global pipes are used across the whole application, for every controller and every route handler.

Note that in terms of dependency injection, global pipes registered from outside of any module (with `useGlobalPipes()` as in the example above) cannot inject dependencies since the binding has been done outside the context of any module. In order to solve this issue, you can set up a global pipe **directly from any module** using the following construction:

```typescript
import { Module } from '@nestjs/common';
import { APP_PIPE } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_PIPE,
      useClass: ValidationPipe,
    },
  ],
})
export class AppModule {}
```

