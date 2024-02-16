# Controller

- **Routing** mechanism: controls which controller receives which requests.
- In nestjs, we use classes and decorators.
  - **Decorators** associate classes with required metadata and enable Nest to create a routing map.
- To define controller, we use `@Controller([prefix: string | string[]])` *class decorator*
  - When adding a prefix, It allows us to group a set of related routes
- `@Get()`, `@Post()`, etc..
  - is a *method decorator*. It always is before method.
  - Maps a specific endpoint for HTTP requests.
  - `@Controller('hello')` + `@Get('world')` -> `GET /hello/world`
  - The Nest routes incoming requests to user-defined method.


## It handles Responses
- Standard mode
  - automatically serialized to JSON
  - Default status code -> We can change the status code by using `@HttpCode()` 
- Library specific
  - `myMethod(@Res() response)`


### Routing Parameter
- Parameterize the routing mechanism
  - Parametersize the action of **some resource**


## How to run a controller in Nest Application
- The controllers always belong to a module.