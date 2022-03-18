## This Week I Learned: AWS Lambda cold/warm starts and dependency injection

# This Week I Learned
...about AWS Lambda cold and warm starts, and the implications they have on dependency injection.

### What is a cold start? What is a warm start?


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1647323388479/ljgcshJXc.png)

A cold start is short-hand for the process of a new AWS Lambda execution environment being spun up. The process includes fetching the image cued up for execution, and the start up of the container itself. Any initialisation of the actual application code will also occur here, after the container is made ready. Once all of this is done, _then_ our lambda is ready to execute.

A warm start occurs when a function is re-invoked soon enough after a preceding invocation that the container was never torn down. Because the container was never torn down, this invocation can skip all of the setup required to spin up during the first invocation, and can go straight to the actual function execution. I haven't seen official documentation, but the lifetime of an idle lambda container _appears_ to be [between 5 and 7 minutes](https://mikhail.io/serverless/coldstarts/aws/intervals/).

Warm starts are generally seen as desirable, after all, they reduce the overall run time of an execution. They potentially reduce costs as well, since code initialisation doesn't have to happen a second time¹. They do however have an interesting effect on the way we think about our DI (dependency injection) service lifetimes. In [another post](https://blog.jessebellingham.com/dependency-injection-and-service-lifetimes-in-net) I wrote about service lifetimes and the practical implications of the different types of lifetimes. It boils down to exactly what circumstances result in a new instance being created for a request for a given dependency.

Turns out that warm starts also have an interesting impact on dependency lifetimes in .NET.

### Warm starts + dependency injection == unexpected behaviour

Say for example that you had a lambda with some DI registration code that looked like this:

```csharp
public IServiceCollection ConfigureServices()
{
    var services = new ServiceCollection();
    services.AddScoped<IUserService, UserService>();
    // ...more service registrations
    return services;
}
```

We know that dependencies registered as lifetime scoped have a new instance created for every new container scope. We also know that in a web application context .NET will create a new container scope automatically for us for every incoming api request.

Armed with that knowledge, do you think that distinct invocations of our lambda under warm start conditions would create a new container scope, and thus a new instance of our `UserService`? If, like me, you assumed that a new scope would be created, you would be wrong. In retrospect, this makes sense, as I believe the container scope per request behaviour is baked into the .NET request pipeline middleware, which we aren't utilising in a lambda function context.

So what is the solution then? Well, if a container scope per _invocation_ is the goal, we can achieve that by manually creating a new scope inside our lambda's function handler like so:

```csharp
public async Task FunctionHandler(ILambdaContext context)
{
    // If you want a scope per invocation, but the configuration never
    // changes, _serviceProvider can be initialised in the constructor
    // so it only happens once
    using var scope = _serviceProvider.CreateScope()
    var userService = scope.ServiceProvider.GetService<IUserService>();
    await userService.DoSomething();
}
```

Now we know for sure that every execution will get a new container scope, and by extension, a new instance of the user service.

---

1. You are not charged for the time spent spinning up the container, but time spent on code initialisation once the container has started is charged ([Operating Lambda: Performance optimization – Part 1](https://aws.amazon.com/blogs/compute/operating-lambda-performance-optimization-part-1/)).