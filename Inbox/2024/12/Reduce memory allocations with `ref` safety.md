---
created: 2024-10-17T17:10:58 (UTC -03:00)
tags: []
source: chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html
author: BillWagner
---

# 

> ## Excerpt
> Learn to remove allocations from your code. Make use of `struct` types for fewer allocations. Use the `ref` and `in` modifiers to avoid copies and enable or disable modification. Use `ref struct` types like `Span` to directly use memory efficiently.

---
## Tutorial: Reduce memory allocations with `ref` safety

-   Article
-   10/13/2023

## In this article

1.  [Explore the starter application](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#explore-the-starter-application)
2.  [Change classes to structs](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#change-classes-to-structs)
3.  [Avoid making copies](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#avoid-making-copies)
4.  [Preserve semantics](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#preserve-semantics)

Often, performance tuning for a .NET application involves two techniques. First, reduce the number and size of heap allocations. Second, reduce how often data is copied. Visual Studio provides great [tools](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/en-us/visualstudio/profiling/dotnet-alloc-tool) that help analyze how your application is using memory. Once you've determined where your app makes unnecessary allocations, you make changes to minimize those allocations. You convert `class` types to `struct` types. You use `ref` safety [features](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/) to preserve semantics and minimize extra copying.

Use [Visual Studio 17.5](https://visualstudio.microsoft.com/) for the best experience with this tutorial. The .NET object allocation tool used to analyze memory usage is part of Visual Studio. You can use [Visual Studio Code](https://code.visualstudio.com/) and the command line to run the application and make all the changes. However, you won't be able to see the analysis results of your changes.

The application you'll use is a simulation of an IoT application that monitors several sensors to determine if an intruder has entered a secret gallery with valuables. The IoT sensors are constantly sending data that measures the mix of Oxygen (O2) and Carbon Dioxide (CO2) in the air. They also report the temperature and relative humidity. Each of these values is fluctuating slightly all the time. However, when a person enters the room, the change a bit more, and always in the same direction: Oxygen decreases, Carbon Dioxide increases, temperature increases, as does relative humidity. When the sensors combine to show increases, the intruder alarm is triggered.

In this tutorial, you'll run the application, take measurements on memory allocations, then improve the performance by reducing the number of allocations. The source code is available in the [samples browser](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/en-us/samples/dotnet/samples/performance-allocations/).

[](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#explore-the-starter-application)

## Explore the starter application

Download the application and run the starter sample. The starter application works correctly, but because it allocates many small objects with each measurement cycle, its performance slowly degrades as it runs over time.

```
<span>Press &lt;return&gt; to start simulation

Debounced measurements:
    Temp:      67.332
    Humidity:  41.077%
    Oxygen:    21.097%
    CO2 (ppm): 404.906
Average measurements:
    Temp:      67.332
    Humidity:  41.077%
    Oxygen:    21.097%
    CO2 (ppm): 404.906

Debounced measurements:
    Temp:      67.349
    Humidity:  46.605%
    Oxygen:    20.998%
    CO2 (ppm): 408.707
Average measurements:
    Temp:      67.349
    Humidity:  46.605%
    Oxygen:    20.998%
    CO2 (ppm): 408.707
</span>
```

Many rows removed.

```
<span>Debounced measurements:
    Temp:      67.597
    Humidity:  46.543%
    Oxygen:    19.021%
    CO2 (ppm): 429.149
Average measurements:
    Temp:      67.568
    Humidity:  45.684%
    Oxygen:    19.631%
    CO2 (ppm): 423.498
Current intruders: 3
Calculated intruder risk: High

Debounced measurements:
    Temp:      67.602
    Humidity:  46.835%
    Oxygen:    19.003%
    CO2 (ppm): 429.393
Average measurements:
    Temp:      67.568
    Humidity:  45.684%
    Oxygen:    19.631%
    CO2 (ppm): 423.498
Current intruders: 3
Calculated intruder risk: High
</span>
```

You can explore the code to learn how the application works. The main program runs the simulation. After you press `<Enter>`, it creates a room, and gathers some initial baseline data:

```
<span>Console.WriteLine(<span>"Press &lt;return&gt; to start simulation"</span>);
Console.ReadLine();
<span>var</span> room = <span>new</span> Room(<span>"gallery"</span>);
<span>var</span> r = <span>new</span> Random();

<span>int</span> counter = <span>0</span>;

room.TakeMeasurements(
    m =&gt;
    {
        Console.WriteLine(room.Debounce);
        Console.WriteLine(room.Average);
        Console.WriteLine();
        counter++;
        <span>return</span> counter &lt; <span>20000</span>;
    });
</span>
```

Once that baseline data has been established, it runs the simulation on the room, where a random number generator determines if an intruder has entered the room:

```
<span>counter = <span>0</span>;
room.TakeMeasurements(
    m =&gt;
    {
        Console.WriteLine(room.Debounce);
        Console.WriteLine(room.Average);
        room.Intruders += (room.Intruders, r.Next(<span>5</span>)) <span>switch</span>
        {
            ( &gt; <span>0</span>, <span>0</span>) =&gt; <span>-1</span>,
            ( &lt; <span>3</span>, <span>1</span>) =&gt; <span>1</span>,
            _ =&gt; <span>0</span>
        };

        Console.WriteLine(<span>$"Current intruders: <span>{room.Intruders}</span>"</span>);
        Console.WriteLine(<span>$"Calculated intruder risk: <span>{room.RiskStatus}</span>"</span>);
        Console.WriteLine();
        counter++;
        <span>return</span> counter &lt; <span>200000</span>;
    });
</span>
```

Other types contain the measurements, a debounced measurement that is the average of the last 50 measurements, and the average of all measurements taken.

Next, run the application using the [.NET object allocation tool](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/en-us/visualstudio/profiling/dotnet-alloc-tool). Make sure you're using the `Release` build, not the `Debug` build. On the _Debug_ menu, open the _Performance profiler_. Check the _.NET Object Allocation Tracking_ option, but nothing else. Run your application to completion. The profiler measures object allocations and reports on allocations and garbage collection cycles. You should see a graph similar to the following image:

![Allocation graph for running the intruder alert app before any optimizations.](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/media/ref-tutorial/profilerallocationgraph.png)

The previous graph shows that working to minimize allocations will provide performance benefits. You see a sawtooth pattern in the live objects graph. That tells you that numerous objects are created that quickly become garbage. They're later collected, as shown in the object delta graph. The downward red bars indicate a garbage collection cycle.

Next, look at the _Allocations_ tab below the graphs. This table shows what types are allocated the most:

![Chart that shows which types are allocated most frequently.](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/media/ref-tutorial/allocationsbytype.png)

The [System.String](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/en-us/dotnet/api/system.string) type accounts for the most allocations. The most important task should be to minimize the frequency of string allocations. This application prints numerous formatted output to the console constantly. For this simulation, we want to keep messages, so we'll concentrate on the next two rows: the `SensorMeasurement` type, and the `IntruderRisk` type.

Double-click on the `SensorMeasurement` line. You can see that all the allocations take place in the `static` method `SensorMeasurement.TakeMeasurement`. You can see the method in the following snippet:

```
<span><span><span>public</span> <span>static</span> SensorMeasurement <span>TakeMeasurement</span>(<span><span>string</span> room, <span>int</span> intruders</span>)</span>
{
    <span>return</span> <span>new</span> SensorMeasurement
    {
        CO2 = (CO2Concentration + intruders * <span>10</span>) + (<span>20</span> * generator.NextDouble() - <span>10.0</span>),
        O2 = (O2Concentration - intruders * <span>0.01</span>) + (<span>0.005</span> * generator.NextDouble() - <span>0.0025</span>),
        Temperature = (TemperatureSetting + intruders * <span>0.05</span>) + (<span>0.5</span> * generator.NextDouble() - <span>0.25</span>),
        Humidity = (HumiditySetting + intruders * <span>0.005</span>) + (<span>0.20</span> * generator.NextDouble() - <span>0.10</span>),
        Room = room,
        TimeRecorded = DateTime.Now
    };
}
</span>
```

Every measurement allocates a new `SensorMeasurement` object, which is a `class` type. Every `SensorMeasurement` created causes a heap allocation.

[](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#change-classes-to-structs)

## Change classes to structs

The following code shows the initial declaration of `SensorMeasurement`:

```
<span><span>public</span> <span>class</span> <span>SensorMeasurement</span>
{
    <span>private</span> <span>static</span> <span>readonly</span> Random generator = <span>new</span> Random();

    <span><span>public</span> <span>static</span> SensorMeasurement <span>TakeMeasurement</span>(<span><span>string</span> room, <span>int</span> intruders</span>)</span>
    {
        <span>return</span> <span>new</span> SensorMeasurement
        {
            CO2 = (CO2Concentration + intruders * <span>10</span>) + (<span>20</span> * generator.NextDouble() - <span>10.0</span>),
            O2 = (O2Concentration - intruders * <span>0.01</span>) + (<span>0.005</span> * generator.NextDouble() - <span>0.0025</span>),
            Temperature = (TemperatureSetting + intruders * <span>0.05</span>) + (<span>0.5</span> * generator.NextDouble() - <span>0.25</span>),
            Humidity = (HumiditySetting + intruders * <span>0.005</span>) + (<span>0.20</span> * generator.NextDouble() - <span>0.10</span>),
            Room = room,
            TimeRecorded = DateTime.Now
        };
    }

    <span>private</span> <span>const</span> <span>double</span> CO2Concentration = <span>409.8</span>; <span>// increases with people.</span>
    <span>private</span> <span>const</span> <span>double</span> O2Concentration = <span>0.2100</span>; <span>// decreases</span>
    <span>private</span> <span>const</span> <span>double</span> TemperatureSetting = <span>67.5</span>; <span>// increases</span>
    <span>private</span> <span>const</span> <span>double</span> HumiditySetting = <span>0.4500</span>; <span>// increases</span>

    <span>public</span> required <span>double</span> CO2 { <span>get</span>; <span>init</span>; }
    <span>public</span> required <span>double</span> O2 { <span>get</span>; <span>init</span>; }
    <span>public</span> required <span>double</span> Temperature { <span>get</span>; <span>init</span>; }
    <span>public</span> required <span>double</span> Humidity { <span>get</span>; <span>init</span>; }
    <span>public</span> required <span>string</span> Room { <span>get</span>; <span>init</span>; }
    <span>public</span> required DateTime TimeRecorded { <span>get</span>; <span>init</span>; }

    <span><span>public</span> <span>override</span> <span>string</span> <span>ToString</span>(<span></span>)</span> =&gt; <span>$""</span><span>"
            Room: {Room} at {TimeRecorded}:
                Temp:      {Temperature:F3}
                Humidity:  {Humidity:P3}
                Oxygen:    {O2:P3}
                CO2 (ppm): {CO2:F3}
            "</span><span>""</span>;
}
</span>
```

The type was originally created as a `class` because it contains numerous `double` measurements. It's larger than you'd want to copy in hot paths. However, that decision meant a large number of allocations. Change the type from a `class` to a `struct`.

Changing from a `class` to `struct` introduces a few compiler errors because the original code used `null` reference checks in a few spots. The first is in the `DebounceMeasurement` class, in the `AddMeasurement` method:

```
<span><span><span>public</span> <span>void</span> <span>AddMeasurement</span>(<span>SensorMeasurement datum</span>)</span>
{
    <span>int</span> index = totalMeasurements % debounceSize;
    recentMeasurements[index] = datum;
    totalMeasurements++;
    <span>double</span> sumCO2 = <span>0</span>;
    <span>double</span> sumO2 = <span>0</span>;
    <span>double</span> sumTemp = <span>0</span>;
    <span>double</span> sumHumidity = <span>0</span>;
    <span>for</span> (<span>int</span> i = <span>0</span>; i &lt; debounceSize; i++)
    {
        <span>if</span> (recentMeasurements[i] <span>is</span> <span>not</span> <span>null</span>)
        {
            sumCO2 += recentMeasurements[i].CO2;
            sumO2+= recentMeasurements[i].O2;
            sumTemp+= recentMeasurements[i].Temperature;
            sumHumidity += recentMeasurements[i].Humidity;
        }
    }
    O2 = sumO2 / ((totalMeasurements &gt; debounceSize) ? debounceSize : totalMeasurements);
    CO2 = sumCO2 / ((totalMeasurements &gt; debounceSize) ? debounceSize : totalMeasurements);
    Temperature = sumTemp / ((totalMeasurements &gt; debounceSize) ? debounceSize : totalMeasurements);
    Humidity = sumHumidity / ((totalMeasurements &gt; debounceSize) ? debounceSize : totalMeasurements);
}
</span>
```

The `DebounceMeasurement` type contains an array of 50 measurements. The readings for a sensor are reported as the average of the last 50 measurements. That reduces the noise in the readings. Before a full 50 readings have been taken, these values are `null`. The code checks for `null` reference to report the correct average on system startup. After changing the `SensorMeasurement` type to a struct, you must use a different test. The `SensorMeasurement` type includes a `string` for the room identifier, so you can use that test instead:

```
<span><span>if</span> (recentMeasurements[i].Room <span>is</span> <span>not</span> <span>null</span>)
</span>
```

The other three compiler errors are all in the method that repeatedly takes measurements in a room:

```
<span><span><span>public</span> <span>void</span> <span>TakeMeasurements</span>(<span>Func&lt;SensorMeasurement, <span>bool</span>&gt; MeasurementHandler</span>)</span>
{
    SensorMeasurement? measure = <span>default</span>;
    <span>do</span> {
        measure = SensorMeasurement.TakeMeasurement(Name, Intruders);
        Average.AddMeasurement(measure);
        Debounce.AddMeasurement(measure);
    } <span>while</span> (MeasurementHandler(measure));
}
</span>
```

In the starter method, the local variable for the `SensorMeasurement` is a _nullable reference_:

```
<span>SensorMeasurement? measure = <span>default</span>;
</span>
```

Now that the `SensorMeasurement` is a `struct` instead of a `class`, the nullable is a _nullable value type_. You can change the declaration to a value type to fix the remaining compiler errors:

```
<span>SensorMeasurement measure = <span>default</span>;
</span>
```

Now that the compiler errors have been addressed, you should examine the code to ensure the semantics haven't changed. Because `struct` types are passed by value, modifications made to method parameters aren't visible after the method returns.

Important

Changing a type from a `class` to a `struct` can change the semantics of your program. When a `class` type is passed to a method, any mutations made in the method are made to the argument. When a `struct` type is passed to a method, and mutations made in the method are made to _a copy_ of the argument. That means any method that modifies its arguments by design should be updated to use the `ref` modifier on any argument type you've changed from a `class` to a `struct`.

The `SensorMeasurement` type doesn't include any methods that change state, so that's not a concern in this sample. You can prove that by adding the `readonly` modifier to the `SensorMeasurement` struct:

```
<span><span>public</span> <span>readonly</span> <span>struct</span> SensorMeasurement
</span>
```

The compiler enforces the `readonly` nature of the `SensorMeasurement` struct. If your inspection of the code missed some method that modified state, the compiler would tell you. Your app still builds without errors, so this type is `readonly`. Adding the `readonly` modifier when you change a type from a `class` to a `struct` can help you find members that modify the state of the `struct`.

[](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#avoid-making-copies)

## Avoid making copies

You've removed a large number of unnecessary allocations from your app. The `SensorMeasurement` type doesn't appear in the table anywhere.

Now, it's doing extra working copying the `SensorMeasurement` structure every time it's used as a parameter or a return value. The `SensorMeasurement` struct contains four doubles, a [DateTime](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/en-us/dotnet/api/system.datetime) and a `string`. That structure is measurably larger than a reference. Let's add the `ref` or `in` modifiers to places where the `SensorMeasurement` type is used.

The next step is to find methods that return a measurement, or take a measurement as an argument, and use references where possible. Start in the `SensorMeasurement` struct. The static `TakeMeasurement` method creates and returns a new `SensorMeasurement`:

```
<span><span><span>public</span> <span>static</span> SensorMeasurement <span>TakeMeasurement</span>(<span><span>string</span> room, <span>int</span> intruders</span>)</span>
{
    <span>return</span> <span>new</span> SensorMeasurement
    {
        CO2 = (CO2Concentration + intruders * <span>10</span>) + (<span>20</span> * generator.NextDouble() - <span>10.0</span>),
        O2 = (O2Concentration - intruders * <span>0.01</span>) + (<span>0.005</span> * generator.NextDouble() - <span>0.0025</span>),
        Temperature = (TemperatureSetting + intruders * <span>0.05</span>) + (<span>0.5</span> * generator.NextDouble() - <span>0.25</span>),
        Humidity = (HumiditySetting + intruders * <span>0.005</span>) + (<span>0.20</span> * generator.NextDouble() - <span>0.10</span>),
        Room = room,
        TimeRecorded = DateTime.Now
    };
}
</span>
```

We'll leave this one as is, returning by value. If you tried to return by `ref`, you'd get a compiler error. You can't return a `ref` to a new structure locally created in the method. The design of the immutable struct means you can only set the values of the measurement at construction. This method must create a new measurement struct.

Let's look again at `DebounceMeasurement.AddMeasurement`. You should add the `in` modifier to the `measurement` parameter:

```
<span><span><span>public</span> <span>void</span> <span>AddMeasurement</span>(<span><span>in</span> SensorMeasurement datum</span>)</span>
{
    <span>int</span> index = totalMeasurements % debounceSize;
    recentMeasurements[index] = datum;
    totalMeasurements++;
    <span>double</span> sumCO2 = <span>0</span>;
    <span>double</span> sumO2 = <span>0</span>;
    <span>double</span> sumTemp = <span>0</span>;
    <span>double</span> sumHumidity = <span>0</span>;
    <span>for</span> (<span>int</span> i = <span>0</span>; i &lt; debounceSize; i++)
    {
        <span>if</span> (recentMeasurements[i].Room <span>is</span> <span>not</span> <span>null</span>)
        {
            sumCO2 += recentMeasurements[i].CO2;
            sumO2+= recentMeasurements[i].O2;
            sumTemp+= recentMeasurements[i].Temperature;
            sumHumidity += recentMeasurements[i].Humidity;
        }
    }
    O2 = sumO2 / ((totalMeasurements &gt; debounceSize) ? debounceSize : totalMeasurements);
    CO2 = sumCO2 / ((totalMeasurements &gt; debounceSize) ? debounceSize : totalMeasurements);
    Temperature = sumTemp / ((totalMeasurements &gt; debounceSize) ? debounceSize : totalMeasurements);
    Humidity = sumHumidity / ((totalMeasurements &gt; debounceSize) ? debounceSize : totalMeasurements);
}
</span>
```

That saves one copy operation. The `in` parameter is a reference to the copy already created by the caller. You can also save a copy with the `TakeMeasurement` method in the `Room` type. This method illustrates how the compiler provides safety when you pass arguments by `ref`. The initial `TakeMeasurement` method in the `Room` type takes an argument of `Func<SensorMeasurement, bool>`. If you try to add the `in` or `ref` modifier to that declaration, the compiler reports an error. You can't pass a `ref` argument to a lambda expression. The compiler can't guarantee that the called expression doesn't copy the reference. If the lambda expression _captures_ the reference, the reference could have a lifetime longer than the value it refers to. Accessing it outside its _ref safe context_ would result in memory corruption. The `ref` safety rules don't allow it. You can learn more in the overview of [ref safety features](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/#ref-safe-context).

[](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#preserve-semantics)

## Preserve semantics

The final sets of changes won't have a major impact on this application's performance because the types aren't created in hot paths. These changes illustrate some of the other techniques you'd use in your performance tuning. Let's take a look at the initial `Room` class:

```
<span><span>public</span> <span>class</span> <span>Room</span>
{
    <span>public</span> AverageMeasurement Average { <span>get</span>; } = <span>new</span> ();
    <span>public</span> DebounceMeasurement Debounce { <span>get</span>; } = <span>new</span> ();
    <span>public</span> <span>string</span> Name { <span>get</span>; }

    <span>public</span> IntruderRisk RiskStatus
    {
        <span>get</span>
        {
            <span>var</span> CO2Variance = (Debounce.CO2 - Average.CO2) &gt; <span>10.0</span> / <span>4</span>;
            <span>var</span> O2Variance = (Average.O2 - Debounce.O2) &gt; <span>0.005</span> / <span>4.0</span>;
            <span>var</span> TempVariance = (Debounce.Temperature - Average.Temperature) &gt; <span>0.05</span> / <span>4.0</span>;
            <span>var</span> HumidityVariance = (Debounce.Humidity - Average.Humidity) &gt; <span>0.20</span> / <span>4</span>;
            IntruderRisk risk = IntruderRisk.None;
            <span>if</span> (CO2Variance) { risk++; }
            <span>if</span> (O2Variance) { risk++; }
            <span>if</span> (TempVariance) { risk++; }
            <span>if</span> (HumidityVariance) { risk++; }
            <span>return</span> risk;
        }
    }

    <span>public</span> <span>int</span> Intruders { <span>get</span>; <span>set</span>; }

    
    <span><span>public</span> <span>Room</span>(<span><span>string</span> name</span>)</span>
    {
        Name = name;
    }

    <span><span>public</span> <span>void</span> <span>TakeMeasurements</span>(<span>Func&lt;SensorMeasurement, <span>bool</span>&gt; MeasurementHandler</span>)</span>
    {
        SensorMeasurement? measure = <span>default</span>;
        <span>do</span> {
            measure = SensorMeasurement.TakeMeasurement(Name, Intruders);
            Average.AddMeasurement(measure);
            Debounce.AddMeasurement(measure);
        } <span>while</span> (MeasurementHandler(measure));
    }
}
</span>
```

This type contains several properties. Some are `class` types. Creating a `Room` object involves multiple allocations. One for the `Room` itself, and one for each of the members of a `class` type that it contains. You can convert two of these properties from `class` types to `struct` types: the `DebounceMeasurement` and `AverageMeasurement` types. Let's work through that transformation with both types.

Change the `DebounceMeasurement` type from a `class` to `struct`. That introduces a compiler error `CS8983: A 'struct' with field initializers must include an explicitly declared constructor`. You can fix this by adding an empty parameterless constructor:

```
<span><span><span>public</span> <span>DebounceMeasurement</span>(<span></span>)</span> { }
</span>
```

You can learn more about this requirement in the language reference article on [structs](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/language-reference/builtin-types/struct#struct-initialization-and-default-values).

The [Object.ToString()](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/en-us/dotnet/api/system.object.tostring#system-object-tostring) override doesn't modify any of the values of the struct. You can add the `readonly` modifier to that method declaration. The `DebounceMeasurement` type is _mutable_, so you'll need to take care that modifications don't affect copies that are discarded. The `AddMeasurement` method does modify the state of the object. It's called from the `Room` class, in the `TakeMeasurements` method. You want those changes to persist after calling the method. You can change the `Room.Debounce` property to return a _reference_ to a single instance of the `DebounceMeasurement` type:

```
<span><span>private</span> DebounceMeasurement debounce = <span>new</span>();
<span>public</span> <span>ref</span> <span>readonly</span> DebounceMeasurement Debounce { <span>get</span> { <span>return</span> <span>ref</span> debounce; } }
</span>
```

There are a few changes in the previous example. First, the _property_ is a readonly property that returns a readonly reference to the instance owned by this room. It's now backed by a declared field that's initialized when the `Room` object is instantiated. After making these changes, you'll update the implementation of `AddMeasurement` method. It uses the private backing field, `debounce`, not the readonly property `Debounce`. That way, the changes take place on the single instance created during initialization.

The same technique works with the `Average` property. First, you modify the `AverageMeasurement` type from a `class` to a `struct`, and add the `readonly` modifier on the `ToString` method:

```
<span><span>namespace</span> <span>IntruderAlert</span>;

<span>public</span> <span>struct</span> AverageMeasurement
{
    <span>private</span> <span>double</span> sumCO2 = <span>0</span>;
    <span>private</span> <span>double</span> sumO2 = <span>0</span>;
    <span>private</span> <span>double</span> sumTemperature = <span>0</span>;
    <span>private</span> <span>double</span> sumHumidity = <span>0</span>;
    <span>private</span> <span>int</span> totalMeasurements = <span>0</span>;

    <span><span>public</span> <span>AverageMeasurement</span>(<span></span>)</span> { }

    <span>public</span> <span>readonly</span> <span>double</span> CO2 =&gt; sumCO2 / totalMeasurements;
    <span>public</span> <span>readonly</span> <span>double</span> O2 =&gt; sumO2 / totalMeasurements;
    <span>public</span> <span>readonly</span> <span>double</span> Temperature =&gt; sumTemperature / totalMeasurements;
    <span>public</span> <span>readonly</span> <span>double</span> Humidity =&gt; sumHumidity / totalMeasurements;

    <span><span>public</span> <span>void</span> <span>AddMeasurement</span>(<span><span>in</span> SensorMeasurement datum</span>)</span>
    {
        totalMeasurements++;
        sumCO2 += datum.CO2;
        sumO2 += datum.O2;
        sumTemperature += datum.Temperature;
        sumHumidity+= datum.Humidity;
    }

    <span><span>public</span> <span>readonly</span> <span>override</span> <span>string</span> <span>ToString</span>(<span></span>)</span> =&gt; <span>$""</span><span>"
        Average measurements:
            Temp:      {Temperature:F3}
            Humidity:  {Humidity:P3}
            Oxygen:    {O2:P3}
            CO2 (ppm): {CO2:F3}
        "</span><span>""</span>;
}
</span>
```

Then, you modify the `Room` class following the same technique you used for the `Debounce` property. The `Average` property returns a `readonly ref` to the private field for the average measurement. The `AddMeasurement` method modifies the internal fields.

```
<span><span>private</span> AverageMeasurement average = <span>new</span>();
<span>public</span>  <span>ref</span> <span>readonly</span> AverageMeasurement Average { <span>get</span> { <span>return</span> <span>ref</span> average; } }
</span>
```

[](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#avoid-boxing)

## Avoid boxing

There's one final change to improve performance. The main program is printing stats for the room, including the risk assessment:

```
<span>Console.WriteLine(<span>$"Current intruders: <span>{room.Intruders}</span>"</span>);
Console.WriteLine(<span>$"Calculated intruder risk: <span>{room.RiskStatus}</span>"</span>);
</span>
```

The call to the generated `ToString` boxes the enum value. You can avoid that by writing an override in the `Room` class that formats the string based on the value of estimated risk:

```
<span><span><span>public</span> <span>override</span> <span>string</span> <span>ToString</span>(<span></span>)</span> =&gt;
    <span>$"Calculated intruder risk: <span>{RiskStatus <span>switch</span>
    {
        IntruderRisk.None =&gt; <span>"None"</span>,
        IntruderRisk.Low =&gt; <span>"Low"</span>,
        IntruderRisk.Medium =&gt; <span>"Medium"</span>,
        IntruderRisk.High =&gt; <span>"High"</span>,
        IntruderRisk.Extreme =&gt; <span>"Extreme"</span>,
        _ =&gt; <span>"Error!"</span>
    }</span>}, Current intruders: <span>{Intruders.ToString()}</span>"</span>;
</span>
```

Then, modify the code in the main program to call this new `ToString` method:

```
<span>Console.WriteLine(room.ToString());
</span>
```

Run the app using the profiler and look at the updated table for allocations.

![Allocation graph for running the intruder alert app after modifications.](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/media/ref-tutorial/final-allocations.png)

You've removed numerous allocations, and provided your app with a performance boost.

[](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#using-ref-safety-in-your-application)

## Using ref safety in your application

These techniques are low-level performance tuning. They can increase performance in your application when applied to hot paths, and when you've measured the impact before and after the changes. In most cases, the cycle you'll follow is:

-   _Measure allocations_: Determine what types are being allocated the most, and when you can reduce the heap allocations.
-   _Convert class to struct_: Many times, types can be converted from a `class` to a `struct`. Your app uses stack space instead of making heap allocations.
-   _Preserve semantics_: Converting a `class` to a `struct` can impact the semantics for parameters and return values. Any method that modifies its parameters should now mark those parameters with the `ref` modifier. That ensures the modifications are made to the correct object. Similarly, if a property or method return value should be modified by the caller, that return should be marked with the `ref` modifier.
-   _Avoid copies_: When you pass a large struct as a parameter, you can mark the parameter with the `in` modifier. You can pass a reference in fewer bytes, and ensure that the method doesn't modify the original value. You can also return values by `readonly ref` to return a reference that can't be modified.

Using these techniques you can improve performance in hot paths of your code.

Collaborate with us on GitHub

The source for this content can be found on GitHub, where you can also create and review issues and pull requests. For more information, see [our contributor guide](https://learn.microsoft.com/contribute/content/dotnet/dotnet-contribute).
