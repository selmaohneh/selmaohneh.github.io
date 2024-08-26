title: "How I explained IoC and DI to our Senior Software Engineer"
header:
  teaser: /assets/images/di-ioc.jpg
---

Inversion of Control and Dependency Injection are two of these big buzz words you always hear when talking about modern software development. At least they were when I started going professional. That’s why I invested quite some time to research that topic. I was surprised our Senior Software Engineer did not…

I had to explain it to him while reviewing some of my code. I still remember his reaction:

> That’s it? That easy? I was completely mystified by that stuff for years and you just explained it to me in a short call?

If even my valued colleague, who was my mentor for the past few years, still didn’t get that concept, I thought it might be worth to write yet another explanation:

# Inversion of Control and Dependency Injection for Dummies!

![di-ioc.jpg](/assets/images/di-ioc.jpg)

Let’s say you want to write a piece of code that tracks how much time you spend in front of your computer. You might come up with a simple API like this:

```csharp
using System;

namespace ComputerTimeTracker
{
    public interface IComputerTimeTracker
    {
        void StartTracking();
        void StopTracking();
        TimeSpan GetTimeSpendInFrontOfComputer();
    }
}
```



If my colleague implemented that interface a few years back it would have looked somehow like that:

```csharp
using System;
using System.Diagnostics;

namespace ComputerTimeTracker
{
    public class ComputerTimeTracker : IComputerTimeTracker
    {
        private readonly Stopwatch _stopWatch;

        public ComputerTimeTracker()
        {
            _stopWatch = new Stopwatch();
        }

        public void StartTracking()
        {
            _stopWatch.Start();
        }

        public void StopTracking()
        {
            _stopWatch.Stop();
        }

        public TimeSpan GetTimeSpendInFrontOfComputer()
        {
            return _stopWatch.Elapsed;
        }
    }
}
```

Simple and straight-forward. The only thing this class needs to do its work is a `Stopwatch` instance.



> The class needs an instance of `Stopwatch` .  
>   
> The class is dependent on a `Stopwatch` .  
>   
> The `Stopwatch` is a dependency of the class `ComputerTimeTracker`.



So far, so good. Now let’s just do a little modification to that code:

```csharp
using System;
using System.Diagnostics;

namespace ComputerTimeTracker
{
    public class ComputerTimeTracker : IComputerTimeTracker
    {
        private readonly Stopwatch _stopWatch;

        public ComputerTimeTracker(Stopwatch stopWatch)
        {
            _stopWatch = stopWatch;
        }

        public void StartTracking()
        {
            _stopWatch.Start();
        }

        public void StopTracking()
        {
            _stopWatch.Stop();
        }

        public TimeSpan GetTimeSpendInFrontOfComputer()
        {
            return _stopWatch.Elapsed;
        }
    }
}
```

The only thing that changed is the constructor.

Instead of *creating *the instance of `Stopwatch` inside the constructor we *inject *it into the constructor.



> We inject the `Stopwatch` into the constructor.  
>   
> We inject the dependency `Stopwatch` into the constructor.  
>   
> We just did a dependency injection.



And I just want to make it clear: That is really all! Dependency injection means nothing more and nothing less than providing a dependency to someone who needs it instead of letting the someone create the dependency itself.

It does not matter whether you inject the dependency via the class constructor, via a property, or via a setter-method. All you do is injecting a dependency.

To quote my colleague again:



> I think we have to discuss this. Now I need to create the `Stopwatch` instance before I can even create an instance of `ComputerTimeTracker`. That sucks. Isn’t that a code smell? Object oriented programming always praises information hiding, but now I have to know about a `Stopwatch` ? When I create the instance inside the constructor I just have to create my `ComputerTimeTracker`and it will handle the rest for me. I don’t want to know the implementation details, I just want to use the class. In summary: The `Stopwatch` should be controlled by the `ComputerTimeTracker`.



Notice how he already used the word *controlled*. I’m just gonna think that thought a little bit further:

In the approach of my colleague we have clear *order of control:*



> The class `ComputerTimeTracker`is controlling the `Stopwatch` .  
>   
> It’s the responsibility of the `ComputerTimeTracker`to control the `Stopwatch` .



Now notice what changed when looking at the approach where we injected the `Stopwatch` :



> We, the users of `ComputerTimeTracker`, are controlling the `Stopwatch` .  
>   
> It’s the user’s responsibility to control the `Stopwatch` .



You might have already noticed it. By injecting the dependency we inverted the order of control. We moved the responsibility to control the `Stopwatch` from the class away to the user of the class. That is inversion of control. And again: That is really all!

Now that the buzz words are de-mystified we can talk about why we might prefer the approach with dependency injection. Let's enhance our sample code a little further:

```csharp
using System;

namespace ComputerTimeTracker
{
    public interface IStopwatch
    {
        void Start();
        void Stop();
        TimeSpan Elapsed { get; }
    }

    public class ComputerTimeTracker : IComputerTimeTracker
    {
        private readonly IStopwatch _stopWatch;

        public ComputerTimeTracker(IStopwatch stopWatch)
        {
            _stopWatch = stopWatch;
        }

        public void StartTracking()
        {
            _stopWatch.Start();
        }

        public void StopTracking()
        {
            _stopWatch.Stop();
        }

        public TimeSpan GetTimeSpendInFrontOfComputer()
        {
            return _stopWatch.Elapsed;
        }
    }
}
```



All I changed is swapping the class dependency `Stopwatch` for an interface dependency `IStopwatch`. That way the class `ComputerTimeTracker`is not depending on the class `Stopwatch` of the .NET framework but loosely coupled to any instance that implements the interface `IStopwatch`. Now to our biggest benefit:

# Unit tests!

All dependencies we inject into a class via an interface can be *mocked*. That is super convenient. Consider the following use case:

- You start the tracking

- You spend 42 hours before the computer

- You stop the tracking

- You check the spend time

Writing a unit test for that use case is hard when the `Stopwatch` instance gets created inside the constructor of `ComputerTimeTracker` . You can’t really replace it. And when you can’t replace it you can’t control the `Stopwatch` . So you would have to wait real 42 hours. With dependency injection you can simply inject a mocked `Stopwatch` into the class, just for the test.  


```csharp
[TestClass]
public class ComputerTimeTrackerTests
{
    [TestMethod]
    public void Spend42HoursBeforeTheComputer()
    {
        var stopwatch = new Mock<IStopwatch>();
        stopwatch.Setup(x => x.Elapsed).Returns(TimeSpan.FromHours(42));

        var systemUnderTest = new ComputerTimeTracker(stopwatch.Object);
        systemUnderTest.StartTracking();

        stopwatch.Verify(x => x.Start(), Times.Once);

        systemUnderTest.StopTracking();

        stopwatch.Verify(x => x.Stop(), Times.Once);

        Assert.AreEqual(TimeSpan.FromHours(42), systemUnderTest.GetTimeSpendInFrontOfComputer());
    }
}
```



# **Conclusion**

Dependency injection and inversion of control are important concepts every developer should know. They are a little hyped and often misunderstood. But if you invest some time to really dig into it you see how dumb easy they actually are.

And why are they so hyped at all? For me just because they enable me to write readable unit tests for my code. This already sells it for me.

But there are more benefits I did not cover here: *Dynamic replacement* of dependencies or usage of an *IoC-Container. *But these are topics for a separate article…

Thanks for reading!

