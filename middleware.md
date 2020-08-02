# Middleware

[Home](index.md)

This middleware pattern is derived from the ASP.NET Core [ApplicationBuilder](https://github.com/aspnet/HttpAbstractions/blob/master/src/Microsoft.AspNetCore.Http/Internal/ApplicationBuilder.cs) class:

```
private static void Main()
{
    IList<Func<Action, Action>> middleware = new List<Func<Action, Action>>();

    middleware.Add(next =>
    {
        return () =>
        {
            Console.Write("A");
            next.DynamicInvoke();
            Console.Write("G");
        };
    });

    middleware.Add(next =>
    {
        return () =>
        {
            Console.Write("B");
            next.DynamicInvoke();
            Console.Write("F");
        };
    });

    middleware.Add(next =>
    {
        return () =>
        {
            Console.Write("C");
            next.DynamicInvoke();
            Console.Write("E");
        };
    });

    Action handler = () => { Console.Write("D"); };

    foreach (var component in middleware.Reverse())
    {
        handler = component(handler);
    }

    handler.Invoke();
}
```

Outputs:

```
ABCDEFG
```

This example demonstrates how to create a class that can be used as custom middleware:

```
public class CustomMiddleware
{
    private readonly Action next;

    public CustomMiddleware(Action next)
    {
        this.next = next ?? throw new ArgumentNullException(nameof(next));
    }

    public void Invoke()
    {
        // execute code before the next middleware in the pipeline.
        this.next.DynamicInvoke();
        // execute code after the next middleware in the pipeline.
    }
}
```

To inject this class into the pipeline:

```
middleware.Add(next => new CustomMiddleware(next).Invoke);
```
