Exception handling is not easy. It can be done in the wrong places, it can be done the wrong way, and it can even be forgotten completely. 

The best we can do to help our fellow developers with that problem is to design our API as bulletproof as possible. 

An exception-less approach via the Result Pattern is a step in that direction.

In this article, I will guide you through a typical software problem I faced multiple times working in the production industry. I will provide some simple examples, share my thoughts, and explain why I prefer the exception-less approach in this use case.

![error](/assets/images/error.jpg)

I often have to deal with actual hardware at my job.

* Opening/Closing valves
* Switching relais
* Retrieving values from temperature sensors
* Moving actuators
* …

## The Manufacturer’s DLL
All these expensive hardwares gets shipped with a piece of software in form of a DLL from the manufacturer.

Let’s say we want to measure the ambient pressure and bought some expensive pressure sensor. The manufacturer provided us with a driver that has the following features:

```csharp
public interface IPressureSensorDriver
{
    void Init();
    void StartMeasurement();
    void StopMeasurement();
    double GetPressure();
}
```

As for most manufacturer DLLs the API is not very convenient. To actually poll the current ambient pressure we have to

1. Call Init()
2. Call StartMeasurement
3. Call StopMeasurement()
4. Call GetPressure()

## Don’t repeat yourself!
Since we don’t want calls repeated all over our solution we will introduce a new class that wraps the manufacturer's driver and acts as a convenient service for us.

```csharp
public class PressureService
{
    private readonly IPressureSensorDriver _driver;

    public PressureService(IPressureSensorDriver driver)
    {
        _driver = driver;
    }

    public double GetPressure()
    {
        _driver.Init();
        _driver.StartMeasurement();
        _driver.StopMeasurement();
        double pressure = _driver.GetPressure();

        return pressure;
    }
}
```

Let’s test our service by polling the pressure multiple times in a row:

```csharp
 public class Program
{
    public static async Task Main(string[] args)
    {
        var driver = new PressureSensorDriver();
        var pressureService = new PressureService(driver);

        for (int i = 0; i < 4; i++)
        {
            double pressure = pressureService.GetPressure();
            Console.WriteLine($"Pressure: {pressure}");
            await Task.Delay(1000);
        }

        Console.ReadKey();
    }
}
```

And of course: As soon as we run the application we’ll get an unhandled exception:

> Exception Unhandled — System.Exception: ‘Connection to Sensor interrupted.’

## What did we do wrong?
We just called a method from another application layer without any exception handling.

The driver class is a third-party code we do not know. Nevertheless we crossed that border without any safety net. We just assumed that the method will work as expected.

> Always handle exceptions on application borders!

This includes:

* Hardware calls no matter the communication protocol
* Web Requests or anything that depends on the internet
* External code, either unknown, undocumented or untested

## The Classic Solution
Since we already found the problem, let’s implement the classic solution for it: a try-catch right in our service:

```csharp
public class PressureService
{
    private readonly IPressureSensorDriver _driver;

    public PressureService(IPressureSensorDriver driver)
    {
        _driver = driver;
    }

    public double GetPressure()
    {
        try
        {
            _driver.Init();
            _driver.StartMeasurement();
            _driver.StopMeasurement();
            double pressure = _driver.GetPressure();

            return pressure;
        }
        catch
        {
            return double.NaN;
        }
    }
}
```
That, of course, will work. All exceptions will be swallowed by our `catch`. If something goes wrong, we simply return a `double.NaN` . We are now exception-less!

But there are some flaws. Let’s look at the API of our service from a user’s view. And with API I simply mean our method `GetPressure()`; its name, its return type and its input arguments. Because that is all the user sees.

* The user has no way to know whether the method throws an exception or not. He has no way to decide whether he has to put that method call in a `try/catch` without looking into it. That sucks.
* The user has no way to know whether the method was a success or not. Even when the method ran without an exception, he has to check whether he got a valid number or `double.NaN`. That sucks, as well.

## The Try Pattern
Let’s tweak our service a little further to see if we can get rid of these flaws:

```csharp
public class PressureService
{
    private readonly IPressureSensorDriver _driver;

    public PressureService(IPressureSensorDriver driver)
    {
        _driver = driver;
    }

    public bool TryGetPressure(out double pressure)
    {
        try
        {
            _driver.Init();
            _driver.StartMeasurement();
            _driver.StopMeasurement();
            pressure = _driver.GetPressure();

            return true;
        }
        catch
        {
            pressure = Double.NaN;

            return false;
        }
    }
}
```
Our method now returns the pressure via an out variable. The return value of the method now is a `bool` indicating success or failure of the method. We also tweaked the name of our method from `GetPressure` to `TryGetPressure`. 

We just implemented the Try Pattern.

## What have we won that way?
Method names starting with `Try` are exception-less! That is the most important part of the Try Pattern.

Just by reading the method’s name the user of our service already knows that he does not need to handle any exceptions when calling it. This sounds so trivial but is a big part of API design:

> If you let the user know what to expect, he will do it the correct way.

The other flaw was eliminated, as well: The user can simply check the returned `bool` flag to see if the method call was a success or not. He does not need to look at the pressure value at all, when the method already returned `false`.

But in my opinion there are some new flaws:

* I don’t like the syntax of out variables. A lot of Clean Code Prophets would sign that immediately. It’s just confusing — everyone expects the outputs of the method left to its name and suddenly there is in output right between the inputs? Just because we can, does not mean we should. Don’t obfuscate your code with all the syntax sugar your language provides. Keep it Simple!
* We only have a single, binary result. But most of the time we want to provide the user of our method with more information. Consider all the reasons our method could fail: The service could not connect to the sensor? The service was connected to the sensor but the connection was lost? The sensor returned a value but it is not plausible? There might be different ways the user wants to handle each of these situations. In some he might schedule a retry, in others he might simply show a descriptive error message.

## The Result Pattern
To even solve these flaws, let’s tweak our service even a little further:

```csharp
public class PressureService
{
    private readonly IPressureSensorDriver _driver;

    public PressureService(IPressureSensorDriver driver)
    {
        _driver = driver;
    }

    public Result<double> GetPressure()
    {
        try
        {
            _driver.Init();
            _driver.StartMeasurement();
            _driver.StopMeasurement();
            pressure = _driver.GetPressure();

            if(pressure < 0)
            {
              return Result.Fail("Pressure is not plausible.");
            }

            return Result.Ok(pressure);
        }
        catch()
        {
            return Result.Fail("Could not retrieve Pressure from Sensor.");
        }
    }
}
```
In this version we don’t return a `bool` but a full `object` of type `Result<double>`.

Before digging deeper into the type `Result`, have a look at the new usage of our service:

```csharp
public static async Task Main(string[] args)
{
    var driver = new PressureSensorDriver();
    var pressureService = new PressureService(driver);

    Result<double> pressure = pressureService.GetPressure();

    if (pressure.IsFailed)
    {
        HandleErrorWhileRetrievingPressure(pressure.Errors);
    }

    Console.WriteLine($"Pressure: {pressure}");

    Console.ReadKey();
}
```
If you review the API and its usage you will notice:

You do not retrieve a `double` but a `Result<double>`. You are forced to think about what you will do if the operation failed. Just by using `Result<double>` you know that there could be problems. You wouldn’t have noticed that if I just threw an exception inside the method, would you? And that is the big benefit of the Result Pattern.

> A good API forces the user to use it correctly! It makes it nearly impossible to forget about error handling.

## The Result Object
The result object simply contains the actual value you are interested in plus any additional information you need — error reasons, success reasons, helper methods like `IsFailed` — be creative!

This pattern is not new; it’s a classic. There are many libraries for the Result Pattern available, so you don’t even have to implement your own `Result` class!

My .NET example used the great project [FluentResults](https://github.com/altmann/FluentResults).

## Conclusion
Just to digest, I will repeat my introduction:

Exception handling is not easy. It can be done in the wrong places, it can be done the wrong way, and it can even be forgotten completely.

The best we can do to help our fellow developers with that problem is to design our API as bulletproof as possible.

An exception-less approach via the Result Pattern is a step in that direction.

Thanks for reading!