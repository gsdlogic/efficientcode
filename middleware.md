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
