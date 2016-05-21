

# Overview

The heart of Dagger2 is the [Component](https://google.github.io/dagger/api/latest/dagger/Component.html).  It is the dependency injection container.

* Components are programmed to ...
  * "provide" (i.e. fully construct a object, satisfying all its dependencies) an instance of a dependency, or
  - "inject" (i.e. given an already-constructed object, inject it with its dependencies).
-



## Combining Components

There are two ways to combine components:

1.  via composition — wherein the collaborating component contributes just "provider" methods.
  * (todo: clarify why this should be the default)
- via inheritance — wherein the child component inherits both "provider" and "injection" methods.
  * use this style only when the child component is *truly* a subtype (think [LSP](https://en.wikipedia.org/wiki/Liskov_substitution_principle)) of the parent.


## Adding "Binding"s to a Component

The set of bindings available to a particular `Component.inject()` call are as follows:

* Any class that you've dropped an `@Inject` on its constructor.
  * limited to just those without scope or the same `@Scope` as the component.
  - think of this as an "implied provides" since it's both construction *and* injection.
- For each module listed in the `@Component`'s `module` attribute,
  * ... all methods annotated as `@Provides` within that `@Module` class.
  * ... (and recursively) all methods annotated as `@Provides` within any `@Module` you name in the `@Module`'s `includes` attribute.
- For each component listed in the `@Component`'s `dependencies` attribute:
  * ... all [provision methods](https://google.github.io/dagger/api/latest/dagger/Component.html#provision-methods) from the component.
  - ... the component, itself (right, going meta: Components are automatically available to be injected)
- Unqualified builders for any included subcomponent

## Special Flavors of Bindings

- Provider or Lazy wrappers for any of the above bindings
- A Provider of a Lazy of any of the above bindings (e.g., Provider<Lazy<CoffeeMaker>>)
- A MembersInjector for any type