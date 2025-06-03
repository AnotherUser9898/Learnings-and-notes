#nestjs #guards

A guard is a class annotated with an `@Injectable` decorator and one which implements the `canActivate` interface.

Guards are preferred over Middlewares for authentication because they have access to the `ExecutionContext` including request, response, handler metadata, class and method info.

A guard is defined as follows: 

```typescript
@Injectable()
class AuthGuard implements CanActivate {
	canActivate(context: ExecutionContext): boolean | Promise<boolean> | Observable<boolean> {
		const req = context.switchToHttp().getRequest<Request>();
		return validateRequest(req);
	}
}
```

The `canActivate` requires the class to implement a `canActivate` method, this method takes a context parameter of type `ExecutionContext` and can return a boolean, a promise or an observable. The method can be made async if necessary.

In this section we will implement a guard that authenticates a user based on the bearer token in the request and then attaches the user object to the request object.

```typescript
@Injectable()
class AuthGuard implements CanActivate {
	async canActivate(context: ExecutionContext) {
		const req = context.switchToHttp().getRequest<Request>();
		const token = req.getToken(req);
		if (!token) {
			throw new UnauthorizedException('User not Authorized');
		}
		try {
		const payload = await this.jwtService.verifyAsync(token,
		{
			secret: this.configService.get('JWT_SECRET'),
		});
		req['user'] = payload;
		return true;
		} catch {
			throw new UnauthorizedException('User not Authorized');
		}
	}
}
```

## Binding Guards

We bind Guards similarly to how we bind other Nest JS features, using the `Use*` decorator.

We bind a pipe to a controller the following way.

```typescript
@Controller('cats')
@UseGuards(AuthGuard)
export class CatsController {
	@Post()
	create(@Body() createCatDto: CreateCatDto) {
		return {createCatDto};
	}
}
```

If you guard does not use any dependencies, you can also pass an in place instance. 

```typescript
@Controller('cats')
@UseGuards(new AuthGuard())
export class CatsController {
	@Post()
	create(@Body() createCatDto: CreateCatDto) {
		return {createCatDto};
	}
}
```

### Binding Guards Globally 

In order to set up a global guard we use the `useGlobalGuards` method of the Nest Application Instance.

```typescript
const app = NestFactory.create(AppModule);
app.useGlobalGuards(new AuthGuard());
```

Do note that if you set up a global guard this way you will not be able to inject any dependencies into the guard, since it's defined outside of any module.

If your guard does need dependencies then you can set up a global guard directly from a module using this construction: 

```typescript
@Module({
	providers: [
	{provide: APP_GUARD, useClass: AuthGuard}
	]
})
export class AppModule {}
```


