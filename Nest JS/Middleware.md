#nestjs #middleware

Nest JS Middleware is a function called before the request handler in Nest JS. Middleware functions have access to request and response object of the request as well as the next function. By default Nest JS middleware is equivalent to Express JS middleware and has access to the same objects a middleware function in Express JS might have.

Nest JS supports both class based middleware as well as functional middleware, let's first look at class based middleware.

## Class based Middleware

Class based middleware in Nest JS is simply a class that has had an `@Injectable` decorator applied to it, and implements the `NestMiddleware` interface.

The `NestMiddleware` interface requires the class to implement a `use` function that takes the `request` and `response` objects as well as the `next` function. The middleware logic is then written inside the `use`.

Here is a code snippet to illustrate the previous points

```typescript
@Injectable()
class LoggerMiddleware implements NestMiddleware {
	use(request: Request, response: Response, next: NextFunction) {
		console.log('Request...');
		next();
	}
}
```

### Dependency Injection 

Nest middleware fully supports dependency injection. Just as with controllers and providers they are able to inject dependencies available within the same module. As usual, this is done through constructor.

### Applying Middleware

Middlewares have no space in the `@Module` decorator. Instead we set them up using the `configure` method of the module class. Modules that include middleware have to implement the `NestMiddleware` interface. Let's set up our previously defined `LoggerMiddleware` inside the App level Module.

```typescript
@Module({
	import: [CatModule]
})
class AppModule implements NestModule {
	configure(consumer: MiddlewareConsumer) {
		consumer
		.apply(LoggerMiddleware)
		.forRoutes('cats');
	}
}
```

In the above example we have set up a `LoggerMiddleware` for the `/cats` route, this middleware will run for all HTTP methods associated with the `/cats` route. We may further restricts a middleware to a particular method by passing in an object with properties path and method instead of a string into the `forRoutes` method. We will utilize the `RequestMethod` Enum provided by Nest JS for this purpose.

```typescript
@Module({
	import: [CatModule]
})
class AppModule implements NestModule {
	configure(consumer: MiddlewareConsumer) {
		consumer
		.apply(LoggerMiddleware)
		.forRoutes({ path: 'cats', method: RequestMethod.GET });
	}
}
```

> The `configure()` method can be made asynchronous using `async/await` (e.g., you can `await` completion of an asynchronous operation inside the `configure()` method body).

### Route Wildcards

Pattern-based routes are also supported by Nest JS middleware. For example we can use the named wildcard(`*splat`)  to match any  combination of characters in a route. In the following example the middleware will be executed for any routes starting with `abcd/` , regardless of the number of character that follow. 

```typescript
.forRoutes({
	path: 'abcd/*splat',
	method: RequestMethod.GET	
});
```

The `'abcd/*'` route path will match `abcd/1`, `abcd/123`, `abcd/abc`, and so on. The hyphen ( `-`) and the dot (`.`) are interpreted literally by string-based paths. However, `abcd/` with no additional characters will not match the route. For this, you need to wrap the wildcard in braces to make it optional:

```typescript
forRoutes({
  path: 'abcd/{*splat}',
  method: RequestMethod.ALL,
});
```

### Middleware Consumer

The `MiddlewareConsumer` is a helper class. It provides several several built-in methods to manage middleware. All of them can simply be chained in the [fluent style](https://en.wikipedia.org/wiki/Fluent_interface). The `forRoutes` method can take a string, multiple string, a `RouteInfo` object like we saw before, a controller class and even multiple controller classes. In most cases you will probably pass a list of controller separated by commas:

```typescript
@Module({
	imports: [CatsModule]
})
class AppModule implements NestModule {
	configure(consumer: MiddlewareConsumer) {
		consumer
		.apply(LoggerMiddleware)
		.forRoutes(CatsController);
	}
}
```

In the above example the `LoggerMiddleware` will run for all the routes defined in the `CatsController` middleware.

### Excluding Routes

At times we may want to exclude certain routes from having the middleware applied. This can be easily achieved by using the `exclude` method. The `exclude` method accepts a single string, multiple strings and a `routeInfo` object to identify the routes to be excluded.

Here is an example:

```typescript
consumer
.apply(LoggerMiddleware)
.exclude({ path: 'cats', method: RequestMethod.POST })
.forRoutes(CatsController)
```

## Functional Middleware

If our middleware does not need functionality like dependency injection then instead of using a class we can define our middleware as a function.

Let's convert our `LoggerMiddleware` class to a functional middleware.

```typescript
function logger(request: Request, response: Response, next: NextFunction) {
	console.log('Request...');
	next();
}
```

and apply it within the `AppModule`.

```typescript
consumer
.apply(logger)
.forRoutes(CatsController);
```

### Multiple middleware

As mentioned above, in order to bind multiple middleware that are executed sequentially, simply provide a comma separated list inside the `apply()` method:

```typescript
consumer.apply(cors(), helmet(), logger).forRoutes(CatsController);
```

### Global Middleware

If we want to bind the middleware to every registered route at once, we can use the `use` method that is supplied by the `INestApplication` instance.

```typescript
const app = NestFactory.create(AppModule);
app.use(logger);
await app.listen(process.env.PORT ?? 3000);
```

> Accessing the DI container in a global middleware is not possible. You can use a [[#Functional Middleware]] instead when using `app.use()`. Alternatively, you can use a class middleware and consume it with `.forRoutes('*')` within the `AppModule` (or any other module).

