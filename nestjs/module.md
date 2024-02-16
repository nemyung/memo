# Module
- is a class annotated with a `@Module()` class decorator.
- The `@Module()` provides *metadata* that **Nest** makes use of to organize the application structore
- Some modules can be used other modules.
- The **Application Module** starts to use other Modules..

![Module graph](./module.png)

## Root module(application module)
- The root module is **starting point** Nest uses to build the application graph
  - application graph: internal data structure that Nest runtime uses to resolve module and provider relationships and dependencies.
- The modules are **strongly recommended as an effective way to organize your component**
- For most applications, the resulting architecture will employ multiple modules, each encapsulating a closely **related set of capabilities**.

## `@Module() decorator options`
- **providers**: providers that will be instantiated by Nest injector and may be shared at least across this module
- **controllers**: controllers defined in this module which have to be instantiated.
- **imports**: the list of imported **modules** that export the **providers** which are required in this module
- **exports**
  - the subset of **providers** provided by this module
  - They should be available in other modules which import this module.
  - You can use either the provider itself or its token(provide value)

- **The module encapsulates providers by default.** This means that it's impossible to inject providers that are neither directly part of the current module nor exported from the imported modules. Thus, you may consider the exported providers from a module as the module's public interface, or API.



## Feature modules
- In application domain, `CatsService` and `CatsController` belong to the same domain. As they are closely related, it makes sence to move them into a feature module.
- A feature module simply organizes code relevant for a specific feature, keeping code organized and establishing clear boundaries.

## Shared modules
- In nest, moudles are **singleton** by default.
- You can share the same instance of any provider between multiple modules
- Every module is automatically a shared module. Once it is created, it can be reused by any module.
- How to use same instance
  1. [Provider module] export the provider by adding it to the module's `exports`.
  2. [Consumer module] can access one of providers by adding exported module to `imports`.


  ## Global modules
  - The Nest runtime encapsulates providers inside the module scope. You aren't able to use a module's providers elsewhere without first importing the encapsulating module.
  - You can make the module global by adding `@Global()` decorator.

  ## Dynamic modules
  - enable developers to easily create customizable modules that can register and configure providers dynamically.
  - `forRoot()` method exposes a collection of providers. The properties returned by dynamic module **extend** the base module metadata defined in the `@Module()` decorator.