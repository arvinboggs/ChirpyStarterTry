---
title: Task Scheduling in ASP.NET with Coravel
date: 2024-06-23 12:00:00 +0800
category: Tutorial
tags: tutorial csharp aspnet
image:
  path: /images/task-scheduling-with-coravel.png
  width: 1280   # in pixels
  height: 720   # in pixels
---

# Task Scheduling in ASP.NET with Coravel
## Introduction
Task scheduling is a critical feature for many web applications. Whether you need to run maintenance scripts, send periodic notifications, or update data, having a reliable scheduler can save a lot of time and effort. In this article, we will explore how to use Coravel, a powerful task scheduling library, to schedule tasks in an ASP.NET application using C#.

## What is Coravel?
From its GitHub page: [Coravel](https://github.com/jamesmh/coravel) is a near-zero config .NET library that makes advanced application features like Task Scheduling, Caching, Queuing, Event Broadcasting, and more a breeze! [Documentation](https://docs.coravel.net).

## Installation
To get started with Coravel, you need to install the package via the command line:

```
dotnet add package coravel
```

## Schedule a Task (Option 1)
You can schedule a task directly in your application's startup file (Program.cs). Here's an example:

``` csharp
public static void Main(string[] args)
{
    var builder = WebApplication.CreateBuilder(args);
    builder.Services.AddScheduler(); // Register Coravel's Scheduler
    var app = builder.Build();

    app.Services.UseScheduler(scheduler =>
    {
        scheduler
            .Schedule(() =>
            {
                Console.WriteLine("Put your code here to run it every 10 seconds");
            })
            .EverySeconds(10);
    });

    // The rest of your startup code here
}
```

## Schedule a Task (Option 2)
Alternatively, you can create a class that implements `Coravel.Invocable` for more complex tasks. Here’s how:

1. Create the Invocable Class

``` csharp
public class MinutelyMaintenance : IInvocable
{
    public Task Invoke()
    {
        Console.WriteLine("Put your scheduled task here");
        return Task.CompletedTask;
    }
}
```

2. Register and Schedule the Task in Program.cs

``` csharp
public static void Main(string[] args)
{
    var builder = WebApplication.CreateBuilder(args);

    builder.Services.AddTransient<MinutelyMaintenance>();
    builder.Services.AddScheduler(); // Register Coravel's Scheduler
    var app = builder.Build();

    app.Services.UseScheduler(scheduler =>
    {
        scheduler
            .Schedule<MinutelyMaintenance>()
            .EveryMinute();
    });

    // The rest of your startup code here
}
``` 

## Schedule a Task (Option 3)
You can also schedule tasks inside a controller, which allows for dynamic scheduling based on user requests.

``` csharp
public class ScheduleController : ControllerBase
{
    private readonly IScheduler _scheduler;

    public ScheduleController(IScheduler scheduler)
    {
        _scheduler = scheduler;
    }

    [HttpGet("Add")]
    public string Add()
    {
        _scheduler.Schedule(() =>
        {
            Console.WriteLine($"{DateTime.Now} scheduled via ScheduleController.Add");
        })
        .EverySeconds(7);

        return DateTime.Now.ToString();
    }
}
```

## Alternatives
In case Coravel is not your cup of tea, here are some alternatives that you may choose from:

- [Quartz.NET](https://www.quartz-scheduler.net) : Quartz.NET is a full-featured, open-source job scheduling system that can be used from the smallest apps to large-scale enterprise systems. [GitHub](https://github.com/quartznet/quartznet).

- [Hangfire](https://www.hangfire.io): An easy way to perform background processing in .NET and .NET Core applications. No Windows Service or separate process is required. Backed by persistent storage. Open and free for commercial use. [GitHub](https://github.com/HangfireIO).

## Conclusion
This article provides an introduction to scheduling tasks in ASP.NET using Coravel. By following these examples, you can easily integrate task scheduling into your application. For more advanced configurations and options, refer to the [Coravel documentation](https://docs.coravel.net) to find the best approach for your specific needs.

## Download the Source Code
You can download the source code from this article on [GitHub](https://github.com/arvinboggs/CoravelTry).