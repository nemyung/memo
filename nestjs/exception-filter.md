# Exception-filter

- In the NestJS runtime, there is an **exceptions layer** which is responsible for processing all unhandled exceptions across an application.
- When an exception is not handled by your app code, it is caught by this layer, which automatically sends an appropriate user-friendly response.

## Throwing standard exceptions
- It is best practice to send standard HTTP response objects when certain conditions occur by using `HttpException` class.
- The built-in Nest Exceptions that inherit from the base `HttpException` class is from the `@nestjs/common` package.
- All the built-in exceptions can also provide both an error `cause` and an error `description`

## Exception Filter
- If you want **full control** over the exception layer, **Exception Filters** are designed for exactly this purpose.
  1. add logging
  2. use different JSON schema based on some dynamic factors.

```ts
import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
      });
  }
}
```

- `@Catch(HttpException)`: binds the required metadata to the exception filter, telling Nest that this particular filter is looking for exceptions of type `HttpException`.

### Binding filters
- by
  - pass an instance
  - pass class reference << DI

- to
  - route handlers
  - controllers
  - global


<br />

- You can register a global-scoped filter directly from any module using the following construction.
```ts
import { Module } from '@nestjs/common';
import { APP_FILTER } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_FILTER,
      useClass: HttpExceptionFilter,
    },
  ],
})
export class AppModule {}

```

