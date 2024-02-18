# Middleware
- is a funciton or injectable class which is called **before** the route handler.
- is just like middleware of express.
- The developers can implement custom middleware in either a function or in a class with an `@Injectable()`. The class should implement `NestMiddleware` interface.


## Applying middleware
1. in the module side
  - The modules which include middlewares have to implement `NestModule`.
  - `configure()` method can be made by either sync or async.
2. in the global
  - If we want to bind middleware to every registered route at once, we can use the `use()` method that is supplied by the `INestApplication` instance.

## DependencyInjection
```ts
@Injectable()
export class StevenService {
  log() {
    console.log('hi');
  }
}

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  constructor(private readonly stevenService: StevenService) {}
  use(req: Request, res: Response, next: NextFunction) {
    this.stevenService.log();
    next();
  }
}




@Module({
  imports: [CatsModule],
  controllers: [AppController],
  providers: [AppService, StevenService],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(LoggerMiddleware).forRoutes('*');
  }
}
```