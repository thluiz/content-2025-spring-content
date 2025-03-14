---
created: 2024-09-30T10:00:58 (UTC -03:00)
tags: []
source: https://devblogs.microsoft.com/visualstudio/break-for-async-user-unhandled-exceptions-in-the-visual-studio-debugger/
author: Anders Sundheim
---

# Break for Async User-Unhandled exceptions in the Visual Studio Debugger - Visual Studio Blog

> ## Excerpt
> In .NET 9, the Visual Studio debugger now tracks exceptions from user-code async methods into non-user code, improving the debugging experience for ASP.NET apps. Learn how this enhancement helps you catch async exceptions across asynchronous boundaries, making debugging faster and more efficient.

---
Before .NET 9, the debugger was unable to track exceptions thrown from user-code async methods into non-user code framework methods, such as ASP.NET middleware. We are pleased to announce that you will now start seeing the debugger stop for these user-unhandled exceptions in your ASP.NET applications, as well as anywhere else this might happen!

[![Image of Exception User-Unhandled screen with the "Break when this exception type is user-unhandled" option checked](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/word-image-250536-1.png)](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/word-image-250536-1.png)

## Summary

Debugging asynchronous code, especially in frameworks like ASP.NET Core, can be tricky due to the potential for exceptions to be thrown across asynchronous boundaries.

Now, the Visual Studio Debugger will automatically break when an async Task method throws an exception back to framework code. This will allow you to easily identify and diagnose issues in your ASP.NET applications, leading to faster debugging cycles and improved productivity. Read below for more details about how user-unhandled exceptions work and how the debugger handles async methods.

**Please note that this is for .NET 9 and newer projects only**.

### Details

The Visual Studio Debugger will enter a break state when exceptions are thrown under three different conditions:

-   First chance exceptions, where exception settings indicate that the debugger should break whenever exceptions of the specified type are thrown.
-   Unhandled exceptions, where the exception is unhandled and no catch handler is found.
-   User-unhandled exceptions, where Just My Code is enabled, and an exception was found to have traveled through user code before a catch handler was found in non-user code.

User-unhandled exceptions are the target of the change to account for async user-unhandled scenarios.

All `async Task<T>` functions in C# compile to a state machine with an implicit `catch` handler that catches all exceptions thrown in the `Task`, sets `IsFaulted`, and adds the Exception to the `AggregateException` in `Task.Exception`.

When a `Task` is “unwrapped”, typically either via the preferred `await` or `.Result`, the stored exception is rethrown to the caller as would happen in a synchronous method and the implicit catch handling is not typically important or observed.

To a debugger, on the other hand, this looks like exceptions are being handled! An exception was thrown, it was caught in “user code” (the compiled result of `async Task<T> DoSomethingAsync(...)`), and any non-user code awaiting that Task will throw the exception again from non-user code. It is important to note that when Just My Code is enabled, the runtime will avoid sending the debugger events for exceptions that were not thrown in user code, significantly improving performance.

Now consider this behavior in a typical ASP.NET MVC Controller, when Just My Code is enabled:

`[HttpPost]`

`public async Task<ActionResult<TodoItem>> PostTodoItem(TodoItem todoItem)`

`{`

`_context.TodoItems.Add(todoItem);`

`await _context.SaveChangesAsync(); // imagine this throws some Exception`

`return CreatedAtAction(nameof(GetTodoItem), new { id = todoItem.Id }, todoItem);`

`}`

If `SaveChangesAsync()` throws an unhandled Exception, it will:

1\. Immediately catch it and fault the Task. The debugger is notified, but its user code throwing and catching, so the process continues.

2\. `async Task<ActionResult<TodoItem>> PostTodoItem` will unwrap the faulted Task, rethrow the Exception, and catch it again. Again, the debugger is notified, but nothing is amiss here (and there might be user code that might eventually await it to catch the exception, we cannot see into the future!)

3\. Whatever non-user library/framework middleware that is awaiting `PostTodoItem` will unwrap that Task and rethrow the exception, but since Just My Code is enabled, the debugger is oblivious – that exception was not thrown from user code and caught in non-user code, it was thrown from non-user code.

Thus, changes were required in the runtime to allow the debugger to indicate that we’d like to keep an eye on a particular exception object, so that if the compiled `catch` handler of a user-code `async Task<T>` method catches an exception, we continue to be notified about that exception object in case it is rethrown in non-user code. That way, if an exception is thrown through an ASP.NET MVC Controller, the debugger can break for user-unhandled.

Limitations

There are some limitations with this approach, notably the fact that the debugger is not actually stopped on the `PostTodoItem` frame in the example above, it is stopped at the frame **below** it, where the exception was rethrown and caught in non-user code:

`App!MyMVCApp.DbContextOptions<TodoContext>.SaveChangesAsync() Line 10`

`App!MyMVCApp.TodoController.PostTodoItem(TodoItem todoItem) Line 5`

`[External Code] <- The debugger will stop here`

This means the frames the exception was thrown from have been unwound past and are not necessarily valid to do variable evaluations on. A GC (Garbage Collection) may have occurred, variables may have been changed, and so on. The debugger will create fake `[Exception]` frames to represent the context in which the exception was originally thrown, and will attempt to save information from the async state machine to evaluate variables as best as it can, but certain things get `nulled` out by the compiler as part of async Task exception handling, notably:

1.  Local reference type variables
2.  Local value types with reference fields, or value types that reference other value types with reference fields.

Parameters and class fields/properties will stay intact, as will the exception itself.

For more information, see the original feature request in the public dotnet runtime repository here: [https://github.com/dotnet/runtime/issues/12488.](https://github.com/dotnet/runtime/issues/12488)

To disable entering break state for async user-unhandled, you can run (in the associated Visual Studio Developer Command Prompt)

`vsregedit set local hklm Debugger\EngineSwitches DisableBreakForAsyncUserUnhandled dword 1`

or otherwise set the indicated key for the target installation of Visual Studio.

Library authors who do not want the debugger to stop on expected exceptions thrown into their functions can use the `[DebuggerDisableUserUnhandledExceptions]` attribute introduced in .NET 9 Preview 7, and either rethrow the exception or call the new `Debugger.BreakForUserUnhandledException(Exception e)` function when the exception is unexpected. You can find the API proposal and discussion for this pair of APIs here: [https://github.com/dotnet/runtime/issues/103105.](https://github.com/dotnet/runtime/issues/103105)

## Thank you!

We appreciate the time you’ve spent reporting issues/suggestions and hope you continue to give us feedback when using Visual Studio on what you like and what we can improve. Your feedback is critical to help us make Visual Studio the best tool it can be! You can share feedback with us via [Developer Community](https://developercommunity.visualstudio.com/home%22%20/t%20%22_blank): report any bugs or issues via [report a problem](https://learn.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio?view=vs-2022) and [share your suggestions](https://developercommunity.microsoft.com/VisualStudio/suggest) for new features or improvements to existing ones.

Stay connected with the Visual Studio team by following us on [YouTube](https://www.youtube.com/@visualstudio), [Twitter](https://twitter.com/VisualStudio), [LinkedIn](https://www.linkedin.com/showcase/microsoft-visual-studio/), [Twitch](https://www.twitch.tv/visualstudio) and on [Microsoft Learn](https://learn.microsoft.com/en-us/visualstudio/?view=vs-2022).

## Author

![Anders Sundheim](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/anders_headshot_001-96x96.png)
