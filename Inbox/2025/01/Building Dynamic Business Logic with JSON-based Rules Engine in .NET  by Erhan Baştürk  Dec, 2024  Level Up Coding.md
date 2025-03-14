---
created: 2025-01-10T09:07:28 (UTC -03:00)
tags: []
source: https://levelup.gitconnected.com/building-dynamic-business-logic-with-json-based-rules-engine-in-net-740813ff4d71
author: Erhan Baştürk
---

# Building Dynamic Business Logic with JSON-based Rules Engine in .NET | by Erhan Baştürk | Dec, 2024 | Level Up Coding

> ## Excerpt
> When developing software projects, we often want the project to exhibit different behaviors under different conditions. For example, in a withdrawal operation, if the amount requested by the user…

---
[

![Erhan Baştürk](https://miro.medium.com/v2/resize:fill:88:88/1*0lXVMoDgN8SY3SHdn6S3WA.jpeg)



](https://erhanbasturk.medium.com/?source=post_page---byline--740813ff4d71--------------------------------)

[

![Level Up Coding](https://miro.medium.com/v2/resize:fill:48:48/1*5D9oYBd58pyjMkV_5-zXXQ.jpeg)



](https://levelup.gitconnected.com/?source=post_page---byline--740813ff4d71--------------------------------)

**When developing software projects**, we often want the project to exhibit **different behaviors under different conditions**. For example, in a **withdrawal operation**, if the amount requested by the user exceeds the amount they can withdraw, we return an error message, whereas if it’s smaller, we perform different actions. Similarly, based on the data received from the user, we may wish to exhibit **conditional behavior**, such as generating “**Document A**” in some cases and “**Document B**” in others. Another example where **conditional statements** are frequently needed is **campaign rules**. In fact, some campaign scenarios can push **conditional statements** to their extreme limits.

To meet these needs, we frequently work with **condition blocks**. Statements like **If-Else**, **Switch-Case**, or the **ternary operator** guide the application to behave differently under varying conditions.

However, sometimes the conditions can become highly complex or change over time. At times, their very existence may also vary. For instance, **campaign processes** are essentially conditional statements that can be either simple or complex. Sometimes they are specific to a certain time period, while at other times, their conditions or outcomes may vary.

When we want to change the **behavior of the applications** we develop, we don’t always want to intervene at the **code level** because changes made at the code level bring a new version of the application. A new version, in turn, brings along a new **deployment process**, which may cause the application to be temporarily unavailable. Although we can minimize this issue with **CI/CD processes**, another problem arises. Changes made at the code level can lead to **structural deterioration** over time. A simple change required today may turn into an undesirable modification by tomorrow, or even a few hours later. Moreover, if a **developer** involved in the development process leaves the team, the situation becomes more difficult for the new developer taking their place.

For all these reasons, we aim to create **dynamic methods** in our development processes whenever possible. Returning to our topic, let’s imagine a **complex campaign**. This campaign might depend on factors such as the **date**, the **number of items in the cart**, the **total cart value**, or even the **number of items in category “A”** within the cart or the **total value of products in this category**. You can foresee that in such a scenario, resorting to **simple if-else blocks** would put us in a very difficult position. Therefore, we need a **campaign engine**.

Let’s say we’ve developed a **successful campaign engine** that involves intensive **conditional statements** and **algorithmic infrastructures** after rigorous effort. We’ve deployed our system, and everything seems to be running smoothly. One day, the **campaign team** approaches you with a request to create a new campaign. Upon inspection, you realize that this new campaign cannot be supported by the campaign mechanics you’ve developed so far.

The team that worked on the **campaign engine** regroups for a new development process. In the **best-case scenario**, there are no changes in the team. In the **worst-case scenario**, things get a bit more complicated because the new team member needs to be introduced to critical areas, such as the **database tables** used by the campaign engine. Let’s assume we successfully navigate these challenges. We make some **code-level changes** to the campaign engine. Perhaps we’ve made things a bit more complicated, but now we can produce the desired campaign. However, as you might imagine, this process gradually becomes more painful over time. We want to **isolate our code** from changes related to the **campaign engine**.

I once worked on a project with similar requirements. The method we used at that time was as follows: We built the **campaign engine** on **SQL Stored Procedures**. The only task we wanted to perform at the **code level** was to call the procedure for finding the relevant campaign. Since we handled the remaining operations within the procedure, it was sufficient to modify the procedure when changes were necessary. We didn’t need to make changes to the application’s code or go through **recompilation** or **redeployment processes**. This method adequately met the needs of **complex campaigns** and **dynamic requirements**. However, it’s not an effective method for every project. In our project, **MSSQL** was chosen as the database, and it was certain that this choice wouldn’t change in the long term. Therefore, we could rely on the procedures and adhere to them strictly. Of course, the first question that comes to mind about this being unsuitable for every project is exactly this: **What happens if you choose a database that doesn’t support features like procedures**? In such cases, you would need to develop an **alternative solution**.

I recently felt the need to use a more **general solution** in another project. The method I chose to use seemed much more manageable in many respects. In the remainder of this article, we will discuss this method together. In this method, we will define our **rules in JSON format**. Our **rule engine** will make various decisions based on the rules we define. We will store our **JSON rules** in the **file system**. In fact, we will even specify the desired response for a given rule within the rule definition itself. This way, if we ever need to make changes, modifying a few fields in the **JSON structure** will suffice without requiring any **code-level changes**. Of course, instead of storing our rule definitions in the file system, we could store them in **alternative ways**. However, in this article, I will proceed with examples based on the **file system**.

![](https://miro.medium.com/v2/resize:fit:875/0*aZFElHj4_SmXsX2e.png)

Although the **introduction** of the article has been somewhat lengthy due to the need to **define the problem** and provide **real-world examples**, we can finally **roll up our sleeves** and start writing some code.

First, we need to **design a rule engine**. Here, we will utilize a **package** that allows us to define rules in **JSON format**. The package we’ll use is the **RulesEngine**, developed by **Microsoft**. It is described as follows:

> “A JSON-based Rules Engine with extensive Dynamic Expression support.”

The **RulesEngine** package can be easily integrated into **.NET projects** via the **NuGet package manager**. If you’d like, you can include it in your project with the following command:

```
<span id="4b65" data-selectable-paragraph="">dotnet add <span>package</span> RulesEngine </span>
```

You can access the relevant **NuGet page** using the following link:

I will start by creating an **ASP.NET Core API project** within a **Solution**. Our rule-based engine will be implemented as a **Class Library** within the same **Solution**. We will then add this **Class Library** as a **dependency** in our **API project**. Our initial structure will look as follows:

![](https://miro.medium.com/v2/resize:fit:598/1*GCxrNyFIFbqGjwqVWLSDXQ.png)

Now, we need a **scenario**. While we will make some modifications to this set of rules in later sections, I will initially proceed with the **scenario** provided in the **RulesEngine documentation**. Let’s assume we have a **campaign scenario**. Based on various conditions, the **payment amount** will receive a **discount** of 10%, 20%, 25%, 30% or 35%. Let’s assume we’ve defined a **JSON structure** as follows:

```
<span id="da7e" data-selectable-paragraph=""><span>[</span><br>  <span>{</span><br>    <span>"WorkflowName"</span><span>:</span> <span>"Discount"</span><span>,</span><br>    <span>"Rules"</span><span>:</span> <span>[</span><br>      <span>{</span><br>        <span>"RuleName"</span><span>:</span> <span>"GiveDiscount10"</span><span>,</span><br>        <span>"SuccessEvent"</span><span>:</span> <span>"10"</span><span>,</span><br>        <span>"ErrorMessage"</span><span>:</span> <span>"One or more adjust rules failed."</span><span>,</span><br>        <span>"ErrorType"</span><span>:</span> <span>"Error"</span><span>,</span><br>        <span>"RuleExpressionType"</span><span>:</span> <span>"LambdaExpression"</span><span>,</span><br>        <span>"Expression"</span><span>:</span> <span>"input1.country == \"india\" AND input1.loyalityFactor &lt;= 2 AND input1.totalPurchasesToDate &gt;= 5000 AND input2.totalOrders &gt; 2 AND input3.noOfVisitsPerMonth &gt; 2"</span><br>      <span>}</span><span>,</span><br>      <span>{</span><br>        <span>"RuleName"</span><span>:</span> <span>"GiveDiscount20"</span><span>,</span><br>        <span>"SuccessEvent"</span><span>:</span> <span>"20"</span><span>,</span><br>        <span>"ErrorMessage"</span><span>:</span> <span>"One or more adjust rules failed."</span><span>,</span><br>        <span>"ErrorType"</span><span>:</span> <span>"Error"</span><span>,</span><br>        <span>"RuleExpressionType"</span><span>:</span> <span>"LambdaExpression"</span><span>,</span><br>        <span>"Expression"</span><span>:</span> <span>"input1.country == \"india\" AND input1.loyalityFactor == 3 AND input1.totalPurchasesToDate &gt;= 10000 AND input2.totalOrders &gt; 2 AND input3.noOfVisitsPerMonth &gt; 2"</span><br>      <span>}</span><span>,</span><br>      <span>{</span><br>        <span>"RuleName"</span><span>:</span> <span>"GiveDiscount25"</span><span>,</span><br>        <span>"SuccessEvent"</span><span>:</span> <span>"25"</span><span>,</span><br>        <span>"ErrorMessage"</span><span>:</span> <span>"One or more adjust rules failed."</span><span>,</span><br>        <span>"ErrorType"</span><span>:</span> <span>"Error"</span><span>,</span><br>        <span>"RuleExpressionType"</span><span>:</span> <span>"LambdaExpression"</span><span>,</span><br>        <span>"Expression"</span><span>:</span> <span>"input1.country != \"india\" AND input1.loyalityFactor &gt;= 2 AND input1.totalPurchasesToDate &gt;= 10000 AND input2.totalOrders &gt; 2 AND input3.noOfVisitsPerMonth &gt; 5"</span><br>      <span>}</span><span>,</span><br>      <span>{</span><br>        <span>"RuleName"</span><span>:</span> <span>"GiveDiscount30"</span><span>,</span><br>        <span>"SuccessEvent"</span><span>:</span> <span>"30"</span><span>,</span><br>        <span>"ErrorMessage"</span><span>:</span> <span>"One or more adjust rules failed."</span><span>,</span><br>        <span>"ErrorType"</span><span>:</span> <span>"Error"</span><span>,</span><br>        <span>"RuleExpressionType"</span><span>:</span> <span>"LambdaExpression"</span><span>,</span><br>        <span>"Expression"</span><span>:</span> <span>"input1.loyalityFactor &gt; 3 AND input1.totalPurchasesToDate &gt;= 50000 AND input1.totalPurchasesToDate &lt;= 100000 AND input2.totalOrders &gt; 5 AND input3.noOfVisitsPerMonth &gt; 15"</span><br>      <span>}</span><span>,</span><br>      <span>{</span><br>        <span>"RuleName"</span><span>:</span> <span>"GiveDiscount35"</span><span>,</span><br>        <span>"SuccessEvent"</span><span>:</span> <span>"35"</span><span>,</span><br>        <span>"ErrorMessage"</span><span>:</span> <span>"One or more adjust rules failed."</span><span>,</span><br>        <span>"ErrorType"</span><span>:</span> <span>"Error"</span><span>,</span><br>        <span>"RuleExpressionType"</span><span>:</span> <span>"LambdaExpression"</span><span>,</span><br>        <span>"Expression"</span><span>:</span> <span>"input1.loyalityFactor &gt; 3 AND input1.totalPurchasesToDate &gt;= 100000 AND input2.totalOrders &gt; 15 AND input3.noOfVisitsPerMonth &gt; 25"</span><br>      <span>}</span><br>    <span>]</span><br>  <span>}</span><br><span>]</span></span>
```

How will we know which result will be returned under which condition? Let’s take the **GiveDiscount10** rule as an example. If the expression specified in the **Expression** field is satisfied, the value we defined in **SuccessEvent** will be returned. That is: if the condition `input1.country == India AND input1.loyalityFactor <= 2 AND input1.totalPurchasesToDate >= 5000 AND input2.totalOrders > 2 AND input3.noOfVisitsPerMonth > 2` is met, the response will be **"10"**.

Our **rule engine** will evaluate all the **rules** defined here and return the appropriate response based on which condition matches the given data. Suppose multiple **rules** are matched. In that case, the engine will process based on the **first matched rule**. Of course, you might want to design a **cascading campaign structure** or choose the one that provides the **highest discount**. Alternatively, imagine you are using this engine in a **document generation system**. Instead of taking the **first match**, you might want to retrieve all **matching rules** and generate all the corresponding **documents**. In such scenarios, you would need access to all the **matching rules**. Later in this article, I will also cover how we can achieve this.

First, we need to define the **object** that will be created as a result of the **evaluation** by our rule engine. Under the **Models** folder of our **Class Library** project, we create a model named **RuleCheckResponse** as shown below. If the rule check is successfully performed, the **SuccessEvent** value of the first matching rule will be available to us. The model is designed using the **Factory Method Design Pattern**. The responsibility for object creation is delegated to **subclasses**, while the base classes abstract the process of creating objects. The static methods **CheckedSuccessfully** and **CheckedOnFailure** act as **Named Constructors**, making the creation of a **RuleCheckResponse** object straightforward and readable.

```
<span id="9ba3" data-selectable-paragraph=""><span>public</span> <span>record</span> <span>RuleCheckResponse</span><br>{<br>    <span>public</span> <span>string</span>? SuccessEvent { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>bool</span> Success { <span>get</span>; <span>set</span>; }<br><br>    <span><span>public</span> <span>static</span> RuleCheckResponse <span>CheckedSuccessfully</span>(<span><span>string</span>? successEvent</span>)</span> =&gt; <span>new</span>() { Success = <span>true</span>, SuccessEvent = successEvent };<br>    <span><span>public</span> <span>static</span> RuleCheckResponse <span>CheckedOnFailure</span>()</span> =&gt; <span>new</span>() { Success = <span>false</span> };<br>}</span>
```

Now we can start developing a **service** to utilize our **rule engine**. Let’s begin by creating a class named **RuleService** in the **Services** folder of our **Class Library** project. This service will be used in all parts of the application that require rule evaluation.

The service will take a **RuleEngine** object via **dependency injection**. This object is the rule engine that contains all the workflows we create. Using **JSON templates**, we will retrieve all workflows, create a **RuleEngine** object encompassing them, and pass this object when adding our service to the **DI Container**. However, these steps will be addressed in the next stages.

For now, let’s focus solely on this service and assume that our rule engine will be provided to us via **dependency injection**.

Here, the **CheckRuleAsync** method will evaluate all the rules within the workflow specified as a parameter using the values it receives as another parameter. It will then return a response of the type **RuleCheckResponse**, which we created in the previous step.

The **RuleResultTree list** is important because it contains all the rules under the workflow we’ve checked, along with their success statuses. By the time the code below has completed execution, all the rules will have been evaluated, and their results will have been assigned to the list.

```
<span id="92c5" data-selectable-paragraph="">List&lt;RuleResultTree&gt; resultList = <span>await</span> _rulesEngine.ExecuteAllRulesAsync(workflowName, inputs);</span>
```

In the subsequent part of this code, if at least one rule is successfully satisfied, the **OnSuccess** callback will be triggered; if no rule is satisfied, the **OnFail** callback will be executed. As mentioned earlier, if multiple rules are satisfied, the result of the **first matched rule** will be returned.

As a result, our service will have the following structure:

```
<span id="29ee" data-selectable-paragraph=""><span><span>public</span> <span>class</span> <span>RuleService</span>(<span>IRulesEngine rulesEngine</span>)</span><br>{<br>    <span>private</span> <span>readonly</span> IRulesEngine _rulesEngine = rulesEngine;<br>    <span><span>public</span> <span>async</span> Task&lt;RuleCheckResponse&gt; <span>CheckRuleAsync</span>(<span><span>string</span> workflowName, <span>dynamic</span>[] inputs</span>)</span><br>    {<br>        List&lt;RuleResultTree&gt; resultList = <span>await</span> _rulesEngine.ExecuteAllRulesAsync(workflowName, inputs);<br>        RuleCheckResponse? response = <span>null</span>;<br>        resultList.OnSuccess((eventName) =&gt;<br>        {<br>            response = RuleCheckResponse.CheckedSuccessfully(eventName);<br>        });<br>        resultList.OnFail(() =&gt;<br>        {<br>            response = RuleCheckResponse.CheckedOnFailure();<br>        });<br><br>        <span>return</span> response ?? <span>throw</span> <span>new</span> ArgumentNullException(<span>"Rule response cannot be equal to null"</span>);<br>    }<br><br>}</span>
```

As mentioned earlier, in some scenarios, we may want to retrieve **all successful rules** or the **SuccessEvents** associated with all successful scenarios. After all, we might aim to apply the campaign that provides the customer with the greatest benefit.

Using the **RuleResultTree list**, we can easily extract this information. Below is a simple example. You can modify the rest of the method as needed based on what you want to achieve. I will proceed with the **standard approach**, which returns the result of the **first matched rule**.

```
<span id="90c7" data-selectable-paragraph=""><br>List&lt;RuleResultTree&gt; succeededResults = resultList.Where(r =&gt; r.IsSuccess).ToList();<br><br>List&lt;RuleResultTree&gt; failedResults = resultList.Where(r =&gt; !r.IsSuccess).ToList();<br><br>List&lt;<span>string</span>&gt; allSuccessEvents = resultList.Where(r =&gt; r.IsSuccess).Select(r =&gt; r.Rule.SuccessEvent).ToList();</span>
```

Now that our service is ready, we need to add it to the DI container and create the **RuleEngine** object it requires. To avoid complicating the **Program.cs** file, we will create a class called **ServiceRegistration** inside the Class Library and write an extension method in it. Of course, before doing that, we should not forget to add the following lines in our class library project in order to use ASP.NET Core APIs.

```
<span id="d9ef" data-selectable-paragraph=""><span>&lt;<span>ItemGroup</span>&gt;</span><br> <span>&lt;<span>FrameworkReference</span> <span>Include</span>=<span>"Microsoft.AspNetCore.App"</span> /&gt;</span><br><span>&lt;/<span>ItemGroup</span>&gt;</span></span>
```

In the **extension method**, to create the **RuleEngine object**, we need to retrieve the workflows from our **JSON files**. We may have multiple JSON files, as we might prefer to store workflows in different files categorized, rather than writing them all in a single large JSON file. So, we will need some **helper methods**.

To begin, I am creating a class called **RuleFileReader** under the **Helpers folder** in the Class Library project. The code is as follows. The helper methods can be briefly explained as:

**GetRuleFiles**: We will store all our rule files in a single folder. This method will scan that folder and return a list of file locations for the rule files.

**GetRuleFromFile**: Using the file path passed as a parameter, this method will read the file and convert it into a list of workflows, then return it.

```
<span id="5afa" data-selectable-paragraph=""><span>public</span> <span>class</span> <span>RuleFileReader</span><br>{<br>    <span>public</span> <span>static</span> List&lt;Workflow&gt;? LoadWorkflowsFromFile(<span>string</span> filePath)<br>    {<br>        <span>var</span> fileData = File.ReadAllText(filePath);<br>        <span>return</span> JsonConvert.DeserializeObject&lt;List&lt;Workflow&gt;&gt;(fileData);<br>    }<br><br>    <span><span>public</span> <span>static</span> <span>string</span>[] <span>GetRuleFiles</span>(<span><span>string</span> folderPath</span>)</span><br>    {<br>        <span>return</span> Directory.GetFiles(folderPath, <span>"*.json"</span>);<br>    }<br>}</span>
```

Now that everything is in place, we can create our **extension method**. This method will receive the **location of the folder** containing the rule files, read all the files there, convert them into a **list of workflows**, and then create a **RuleEngine** object. It will pass the created object to the **RuleService** as a **dependency injection**. In the final step, **RuleService** will be registered as a **Singleton** in the **DI container**.

```
<span id="d3b8" data-selectable-paragraph=""><span>public</span> <span>static</span> <span>class</span> <span>ServiceRegistration</span><br>{<br>    <span><span>public</span> <span>static</span> <span>void</span> <span>AddRulesEngine</span>(<span><span>this</span> IServiceCollection services, <span>string</span> rulesFolder</span>)</span><br>    {<br>        services.AddSingleton&lt;RuleService&gt;(sp =&gt;<br>        {<br>            <span>string</span>[] ruleFiles = RuleFileReader.GetRuleFiles(rulesFolder);<br>            List&lt;Workflow&gt;? workflows = ruleFiles.SelectMany(filePath =&gt; RuleFileReader.LoadWorkflowsFromFile(filePath) ?? []).ToList();<br>            IRulesEngine ruleEngine = <span>new</span> RulesEngine.RulesEngine([.. workflows]);<br><br>            <span>return</span> <span>new</span> RuleService(ruleEngine);<br>        });<br>    }<br>}</span>
```

Now, everything is almost ready. Let’s create a folder named **Rules** inside our API project and save our rule file in JSON format with a name like **Campaign.json**. After that, we can make our service usable by adding the following code to **Program.cs**. Note that we are passing the location of the **Rules** folder as a parameter.

```
<span id="30fc" data-selectable-paragraph="">builder.Services.AddRulesEngine(Path.Combine(Directory.GetCurrentDirectory(), <span>"Rules"</span>));</span>
```

In the final stage, you can review the following code for a usage example. We are simply adding our inputs into an array of type `dynamic`. The first input added to the array is represented as **input1** in the JSON file, the second input as **input2**, and so on. We are adding our service through dependency injection. When calling the **CheckRuleAsync** method, we pass the name of the workflow we want to check along with the dynamic array. That’s all for the process.

```
<span id="b32b" data-selectable-paragraph=""><span><span>public</span> <span>class</span> <span>OrdersController</span>(<span>RuleService ruleService</span>) : ControllerBase</span><br>{<br>    <span>private</span> <span>readonly</span> RuleService _ruleService = ruleService;<br><br>    [<span>HttpPost</span>]<br>    <span><span>public</span> <span>async</span> Task&lt;IActionResult&gt; <span>PostAsync</span>(<span>OrderCreateRequestDto dto</span>)</span><br>    {<br>        <span>var</span> userDetails = <span>new</span><br>        {<br>            Country = <span>"Turkiye"</span>,<br>            LoyalityFactor = <span>2</span>,<br>            TotalPurchasesToDate = <span>20000</span><br>        };<br>        <span>var</span> visitDetails = <span>new</span><br>        {<br>            NoOfVisitsPerMonth = <span>20</span><br>        };<br>        <span>dynamic</span>[] inputs = [userDetails, dto, visitDetails];<br>        RuleCheckResponse response = <span>await</span> _ruleService.CheckRuleAsync(<span>"Discount"</span>, inputs);<br>        <span>if</span> (response.Success &amp;&amp; response.SuccessEvent != <span>null</span>)<br>        {<br>            <span>return</span> Ok(<span>$"You have earned a <span>{response.SuccessEvent}</span> percent discount"</span>);<br>        }<br><br>        <span>return</span> Ok(<span>"You have not earn a discount"</span>);<br>    }<br>}</span>
```

I want to address one more point. Let’s say you need to write a condition that is too complex to express in the JSON file. You need to write your own custom method, and you want the rule engine to call that method. This is also possible. Let’s examine this with a simple example.

We will create a **CustomMethods** folder inside the Class Library. This folder will contain the custom methods we write for specific purposes. Then, let’s create a static class named **CampaignMethods** and write a simple method inside it. You can see the code structure below.

```
<span id="9b6c" data-selectable-paragraph=""><span>public</span> <span>static</span> <span>class</span> <span>CampaignMethods</span><br>{<br>    <span>private</span> <span>static</span> Dictionary&lt;<span>string</span>, <span>int</span>&gt; CountriesAndTotalOrders = <span>new</span>()<br>    {<br>        { <span>"Turkiye"</span>, <span>3</span> },<br>        { <span>"USA"</span>, <span>2</span> },<br>        { <span>"UK"</span>, <span>1</span> }<br>    };<br>    <span><span>public</span> <span>static</span> <span>bool</span> <span>CustomDiscountMethod</span>(<span><span>string</span> country, <span>int</span> totalOrders</span>)</span><br>    {<br>        <span>if</span>(CountriesAndTotalOrders.TryGetValue(country, <span>out</span> <span>int</span> <span>value</span>))<br>        {<br>            <span>return</span> totalOrders &gt;= <span>value</span>;<br>        };<br><br>        <span>return</span> <span>false</span>;<br>    }<br>}</span>
```

This is not a very meaningful method, but what’s important for us is that it can be called with the **JSON definition**. When generating the **RuleEngine** object, we’ll need to make some special adjustments. For example, it needs to **recognize this class**. Every time we create a new class, we need to **introduce it again**. However, we can avoid this hassle by using **reflection**. We will write an additional method that uses reflection to find all **static classes** under the **CustomMethods** folder and register them automatically.

Let’s go to our **extension method** and update it as shown below. The **GetCustomTypes** method will do the operations mentioned above and return the classes it finds as an array. Now, all we need to do is provide a **ReSettings object** to the **RuleEngine** that introduces our custom methods.

```
<span id="d794" data-selectable-paragraph=""><span>public</span> <span>static</span> <span>class</span> <span>ServiceRegistration</span><br>{<br>    <span><span>public</span> <span>static</span> <span>void</span> <span>AddRulesEngine</span>(<span><span>this</span> IServiceCollection services, <span>string</span> rulesFolder</span>)</span><br>    {<br>        services.AddSingleton&lt;RuleService&gt;(sp =&gt;<br>        {<br>            <span>string</span>[] ruleFiles = RuleFileReader.GetRuleFiles(rulesFolder);<br>            List&lt;Workflow&gt;? workflows = ruleFiles.SelectMany(filePath =&gt; RuleFileReader.LoadWorkflowsFromFile(filePath) ?? []).ToList();<br>            ReSettings reSettingsWithCustomTypes = <span>new</span>()<br>            {<br>                CustomTypes = GetCustomTypes()<br>            };<br>            IRulesEngine ruleEngine = <span>new</span> RulesEngine.RulesEngine([.. workflows], reSettingsWithCustomTypes);<br><br>            <span>return</span> <span>new</span> RuleService(ruleEngine);<br>        });<br>    }<br><br>    <span><span>private</span> <span>static</span> Type[] <span>GetCustomTypes</span>()</span><br>    {<br>        <span>var</span> assembly = Assembly.GetExecutingAssembly();<br>        <span>var</span> targetNamespace = <span>typeof</span>(CampaignMethods).Namespace;<br>        <span>var</span> types = assembly.GetTypes()<br>            .Where(t =&gt;<br>                t.IsClass &amp;&amp;<br>                t.IsAbstract &amp;&amp;<br>                t.IsSealed &amp;&amp;<br>                t.Namespace == targetNamespace<br>            );<br><br>        <span>return</span> [.. types];<br>    }<br>}</span>
```

I’m adding an example rule to the **JSON file** as shown below. By reviewing it, you can easily see how we are using our **custom method**.

```
<span id="e8d1" data-selectable-paragraph=""><span>{</span><br>    <span>"RuleName"</span><span>:</span> <span>"GiveDiscount40"</span><span>,</span><br>    <span>"SuccessEvent"</span><span>:</span> <span>"40"</span><span>,</span><br>    <span>"ErrorMessage"</span><span>:</span> <span>"One or more adjust rules failed."</span><span>,</span><br>    <span>"ErrorType"</span><span>:</span> <span>"Error"</span><span>,</span><br>    <span>"RuleExpressionType"</span><span>:</span> <span>"LambdaExpression"</span><span>,</span><br>    <span>"Expression"</span><span>:</span> <span>"input1.loyalityFactor &gt; 4 AND input1.totalPurchasesToDate &gt;= 200000 AND CampaignMethods.CustomDiscountMethod(input1.country, input2.totalOrders) == true"</span><br><span>}</span></span>
```

**The RulesEngine package** offers many **customization options**. You can access its **documentation** through this link.

## **CONCLUSION**

**RulesEngine** is helpful in many scenarios where rule-based processing or workflow creation is needed. Definitions can be stored in the **file system**, as mentioned earlier, or in various other ways. In addition to being extendable with **custom methods**, it provides many features on its own, as seen in the Microsoft documentation. However, an important consideration during its use is the **limited debugging capabilities**. Since expressions and their parameters are dynamically passed, great care must be taken when defining and using them to ensure **type-safety**.

You can access the **Git repository** containing the code discussed in the article through the following link:

[https://github.com/basturkerhan/rulesengine-medium](https://github.com/basturkerhan/rulesengine-medium)
