## Dependency injection & service lifetimes in .NET

# Service lifetimes
Since you're here reading this, I'm going to assume you already know what Dependency Injection (DI) is, so I won't bother explaining it here. If you need a refresher, [here is a decent explanation](https://en.wikipedia.org/wiki/Dependency_injection). What you may be less familiar with is service lifetimes. Service lifetimes are important to consider when building your applications, because it can have performance implications, and it can even be the source of some unexpected behaviour.

Configuring DI containers is often referred to as "registering" dependencies. Broadly, there are three types of service lifetimes you can register your dependencies with:

### Singleton

You may have seen the concept of the singleton around before, it doesn't mean anything different in the DI context. Registering a dependency as a singleton means that while the application is running, there can only ever be one instance of this dependency. There are design implications that go into registering a dependency as a singleton that I won't go into here.

Here is the lifecycle of a dependency registered as singleton during a normal web api request:


![singleton.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1647339547722/Rhrz5L3up.png)

### Scoped

Where singleton dependencies can only ever have one instance, registering a dependency as scoped lifetime means that it will create a brand new instance for every container scope that gets created. So what does that mean? What is a container scope? A container scope refers to a scope in which a particular DI configuration is made available for use. Container scopes can be created manually, e.g.:

```csharp
public void ExampleMethod()
{
    var serviceCollection = ConfigureServices();
    var serviceProvider = serviceCollection.BuildServiceProvider();
    using (var scope = serviceProvider.CreateScope())
    {
        // The variable "scope" is a reference
        // to our newly created container scope

        // We can pull a dependency out of our
        // container that we "registered" during
        // ConfigureServices()
        var dependency = scope.ServiceProvider.GetService<IDependency>();
        dependency.DoSomething();
    }

    serviceCollection = ConfigureDifferentServices();
    serviceProvider = serviceCollection.BuildServiceProvider();
    using (var scope = serviceProvider.CreateScope())
    {
        // The variable "scope" here creates a brand new
        // separate container scope, and can have entirely
        // different service registrations.

        // Because the registration for IDependency inside
        // this container could in theory be completely
        // different, our variable "dependency" could easily
        // resolve to a completely different concrete implementation
        // than in the previous container
        var dependency = scope.ServiceProvider.GetService<IDependency>();
        dependency.DoSomething();
    }
}
```

In .NET a new container scope is automatically created under the hood for every distinct web api request that is received. This means that for each api request, any dependencies registered as lifetime scoped will have a new instance created instead of sharing the same one. Scoped is probably the most common lifetime, as it strikes a good balance between minimising things like shared state and possible memory leaks, and the performance implications of going fully transient.

Here is the lifecycle of a dependency registered as scoped during a normal web api request. Note that when drawn out like this it looks very similar to the singleton lifetime, only the conditions under which a new instance is created changes:

![scoped.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1647339560299/ufJ_R_iL8.png)

### Transient

This is maybe the least intuitive lifetime to think about. Where scoped dependencies will be instantiated for each new container scope, lifetime transient dependencies are instantiated on every request for that dependency. What do I mean by that? Well, when we are building an application with DI, a common method of requesting dependencies is through constructor injection. Something like this: 

```csharp
public class SomeClass
{
    private readonly IDependency _dependency;

    public SomeClass(IDependency dependency)
    {
        _dependency = dependency;
    }
}
```

The reference to `IDependency` in the constructor is a _request_ for that dependency. So if `IDependency` was registered as lifetime transient, this class would get a new instance of whatever concrete class was specified by the registration. If a different class also referenced `IDependency` in its constructor, this is considered a separate request and that class would get its own new copy.

Here is the lifecycle of a dependency registered as transient during a normal web api request.

![transient.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1647339438581/JJ9K5m-R9.png)

Drawn out like this, transient lifetime seems much less complicated than the others.

As this lifetime requires a new instance be constructed for every time it is referenced, this naturally will incur a performance hit. It is best used with lightweight, stateless services that can be constructed fairly simply.

Different languages and frameworks that allow dependency injection may use slightly different names, e.g. scoped lifetime is sometimes called request lifetime. They tend to have similar practical differences however.