---
created: 2024-09-26T11:08:45 (UTC -03:00)
tags: [webdev,beginners,javascript,programming,software,coding,development,engineering,inclusive,community]
source: https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest
author: 
---

# Make Your App Feel Better: 10 UI Tips for Beginner/Mid-Level Developers - DEV Community

> ## Excerpt
> Hey, devs! 👋  As a beginner or mid-level developer, you've probably focused a lot on making your app...

---
Hey, devs! 👋

As a beginner or mid-level developer, you've probably focused a lot on making your app functional. But have you considered how it feels to use? In this post, we'll explore 9 often-overlooked aspects of UI design that can significantly improve your app's user experience.

**Note:** The examples use ReactJS and TailwindCSS for example but these principles can be applied anywhere.

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#1-mind-your-margins-and-padding)1\. Mind Your Margins and Padding

Uneven margins and padding are a dead giveaway that someone’s still getting the hang of design. Consistent spacing makes everything look cleaner and more professional.

### [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#quick-fix)Quick Fix:

Stick to multiples of 4 for margins and padding (think 4px, 8px, 12px, 16px). It gives your layout a nice rhythm and looks way more polished.  

```
<span>.container</span> <span>{</span>
  <span>padding</span><span>:</span> <span>16px</span><span>;</span>
<span>}</span>

<span>.button</span> <span>{</span>
  <span>padding</span><span>:</span> <span>8px</span> <span>12px</span><span>;</span>
<span>}</span>
```

You don’t need to reinvent the wheel—just keep things even, and it’ll instantly look better.

[![Tip #1](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9nnqwerlcw06p26ct7gr.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9nnqwerlcw06p26ct7gr.png)

___

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#2-handle-empty-states-like-a-boss)2\. Handle Empty States Like a Boss

Ever opened an app and been met with a blank screen? Yeah, that’s what happens when devs forget to design for the empty state. When there’s no data to show, don’t leave users hanging.

### [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#pro-tip)Pro Tip:

Add helpful messages or call-to-action (CTA) buttons to guide users on what to do next.  

```
<span>function</span> <span>TodoList</span><span>({</span> <span>todos</span> <span>})</span> <span>{</span>
  <span>if </span><span>(</span><span>todos</span><span>.</span><span>length</span> <span>===</span> <span>0</span><span>)</span> <span>{</span>
    <span>return </span><span>(</span>
      <span>&lt;</span><span>div</span> <span>className</span><span>=</span><span>"flex h-44 flex-col items-center justify-center rounded border bg-white shadow-sm"</span><span>&gt;</span>
        <span>&lt;</span><span>p</span> <span>className</span><span>=</span><span>"mb-2 text-gray-600"</span><span>&gt;</span>Nothing here yet! Why not add your first task?<span>&lt;/</span><span>p</span><span>&gt;</span>
        <span>&lt;</span><span>Button</span> <span>className</span><span>=</span><span>"mt-2 bg-green-500 hover:bg-green-600"</span><span>&gt;</span>
          <span>&lt;</span><span>Plus</span> <span>className</span><span>=</span><span>"mr-2 h-4 w-4"</span> <span>/&gt;</span>
          Add Todo
        <span>&lt;/</span><span>Button</span><span>&gt;</span>
      <span>&lt;/</span><span>div</span><span>&gt;</span>
    <span>);</span>
  <span>}</span>

  <span>// Render todo list</span>
<span>}</span>
```

Turn an empty screen into an opportunity to engage your users!

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#)![Tip #2](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fygjilkimgwk2770j66ll.png)

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#3-always-show-loading-states)3\. Always Show Loading States

If your app is doing _something_ in the background, let your users know. Nothing is worse than wondering if a button click even worked or if the app crashed.

### [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#pro-tip)Pro Tip:

Use a delayed loading spinner for quick operations, so it doesn’t flash too quickly. Here’s a little trick to add a delay:  

```
<span>function</span> <span>useDelayedLoading</span><span>(</span><span>isLoading</span><span>,</span> <span>delay</span> <span>=</span> <span>200</span><span>)</span> <span>{</span>
  <span>const</span> <span>[</span><span>showLoading</span><span>,</span> <span>setShowLoading</span><span>]</span> <span>=</span> <span>useState</span><span>(</span><span>false</span><span>);</span>

  <span>useEffect</span><span>(()</span> <span>=&gt;</span> <span>{</span>
    <span>if </span><span>(</span><span>isLoading</span><span>)</span> <span>{</span>
      <span>const</span> <span>timer</span> <span>=</span> <span>setTimeout</span><span>(()</span> <span>=&gt;</span> <span>setShowLoading</span><span>(</span><span>true</span><span>),</span> <span>delay</span><span>);</span>
      <span>return </span><span>()</span> <span>=&gt;</span> <span>clearTimeout</span><span>(</span><span>timer</span><span>);</span>
    <span>}</span>
    <span>setShowLoading</span><span>(</span><span>false</span><span>);</span>
  <span>},</span> <span>[</span><span>isLoading</span><span>,</span> <span>delay</span><span>]);</span>

  <span>return</span> <span>showLoading</span><span>;</span>
<span>}</span>

<span>// Usage</span>
<span>const</span> <span>showLoading</span> <span>=</span> <span>useDelayedLoading</span><span>(</span><span>isLoading</span><span>);</span>
```

That way, users don’t see the spinner for super quick tasks, but they’ll get feedback when it’s needed.

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#)![Tip #3](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fotu7pfkoxnjh8nam7ucc.png)

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#4-keep-a-clear-visual-hierarchy)4\. Keep a Clear Visual Hierarchy

Not everything on your screen has the same importance, right? Make sure your UI reflects that. Use different font sizes, colors, and weights to guide users through the content.

### [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#pro-tip)Pro Tip:

Stick to 2-3 font sizes for your main content, and use color sparingly to emphasize key points.  

```
<span>.primary-text</span> <span>{</span>
  <span>font-size</span><span>:</span> <span>18px</span><span>;</span>
  <span>color</span><span>:</span> <span>#333</span><span>;</span>
  <span>font-weight</span><span>:</span> <span>bold</span><span>;</span>
<span>}</span>

<span>.secondary-text</span> <span>{</span>
  <span>font-size</span><span>:</span> <span>16px</span><span>;</span>
  <span>color</span><span>:</span> <span>#666</span><span>;</span>
<span>}</span>

<span>.tertiary-text</span> <span>{</span>
  <span>font-size</span><span>:</span> <span>14px</span><span>;</span>
  <span>color</span><span>:</span> <span>#999</span><span>;</span>
<span>}</span>
```

Don’t overcomplicate things—just make sure the user knows what’s important at first glance.

[![Tip #4](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fb9q62dgkp7el8aoxcrok.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fb9q62dgkp7el8aoxcrok.png)

___

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#5-embrace-whitespace-dont-fear-it)5\. Embrace Whitespace, Don’t Fear It

You don’t have to fill every inch of the screen with something. Whitespace (aka negative space) is your friend! It helps your app breathe and makes it easier for users to focus.

### [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#pro-tip)Pro Tip:

Increase space between unrelated elements and decrease it between related ones. It’s all about balance.  

```
<span>.section</span> <span>{</span>
  <span>margin-bottom</span><span>:</span> <span>32px</span><span>;</span> <span>/* Big gap between sections */</span>
<span>}</span>

<span>.section-title</span> <span>{</span>
  <span>margin-bottom</span><span>:</span> <span>16px</span><span>;</span> <span>/* Smaller gap within a section */</span>
<span>}</span>

<span>.list-item</span> <span>{</span>
  <span>margin-bottom</span><span>:</span> <span>8px</span><span>;</span> <span>/* Tight gap between list items */</span>
<span>}</span>
```

Whitespace isn’t wasted space—it’s part of the design!

[![Tip #5](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3u97tnem183fxqpw5ixx.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3u97tnem183fxqpw5ixx.png)

___

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#6-handle-errors-gracefully)6\. Handle Errors Gracefully

No one likes errors, but they happen. When they do, be helpful. Instead of just saying “Error 404,” give the user context and offer ways to fix it or try again.

### [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#pro-tip)Pro Tip:

Make your error messages friendly, clear, and actionable.  

```
<span>function</span> <span>ErrorMessage</span><span>()</span> <span>{</span>
  <span>return </span><span>(</span>
    <span>&lt;</span><span>div</span> <span>className</span><span>=</span><span>"rounded border bg-neutral-10 p-6"</span><span>&gt;</span>
      <span>&lt;</span><span>h3</span> <span>className</span><span>=</span><span>"text-neutral-8900 mb-2 text-xl font-bold"</span><span>&gt;</span>Oops! Page Not Found<span>&lt;/</span><span>h3</span><span>&gt;</span>
      <span>&lt;</span><span>p</span> <span>className</span><span>=</span><span>"mb-4 text-neutral-700"</span><span>&gt;</span>
        We couldn<span>&amp;apos;</span>t find the page you<span>&amp;apos;</span>re looking for.
      <span>&lt;/</span><span>p</span><span>&gt;</span>
      <span>&lt;</span><span>div</span> <span>className</span><span>=</span><span>"flex flex-row-reverse justify-start gap-2"</span><span>&gt;</span>
        <span>&lt;</span><span>Button</span> <span>className</span><span>=</span><span>"rounded"</span> <span>variant</span><span>=</span><span>"default"</span><span>&gt;</span>
          Retry
        <span>&lt;/</span><span>Button</span><span>&gt;</span>
        <span>&lt;</span><span>Button</span> <span>className</span><span>=</span><span>"rounded"</span> <span>variant</span><span>=</span><span>"ghost"</span><span>&gt;</span>
          <span>&lt;</span><span>ArrowLeft</span> <span>className</span><span>=</span><span>"mr-2 h-4 w-4"</span> <span>/&gt;</span>
          Go Back
        <span>&lt;/</span><span>Button</span><span>&gt;</span>
      <span>&lt;/</span><span>div</span><span>&gt;</span>
    <span>&lt;/</span><span>div</span><span>&gt;</span>
  <span>);</span>
<span>}</span>
```

Help users feel like they’re not stuck when things go wrong.

[![Tip #6](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fu971wzx280gw0w6h67u6.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fu971wzx280gw0w6h67u6.png)

___

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#7-add-tooltips-for-long-text-amp-disabled-states)7\. Add Tooltips for Long Text & Disabled States

Ever tried to read something in an app that was cut off or tried to interact with a disabled button, only to be left guessing why? This can be frustrating for users, and it’s easily avoidable. Add tooltips (`title` attribute) to long text or for disabled elements to provide more context.

### [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#pro-tip)Pro Tip:

When text is truncated or a button is disabled, a tooltip can offer users additional information, like what the full text is or why the button is inactive. This keeps the experience smooth and informative.  

```
<span>// Long text tooltip example</span>
  <span>&lt;</span><span>TooltipProvider</span><span>&gt;</span>
    <span>&lt;</span><span>Tooltip</span><span>&gt;</span>
      <span>&lt;</span><span>TooltipTrigger</span> <span>asChild</span><span>&gt;</span>
        <span>&lt;</span><span>p</span>
          <span>className</span><span>=</span><span>"w-48 truncate"</span>
          <span>title</span><span>=</span><span>"This is a very long text that gets cut off but has a helpful tooltip"</span>
        <span>&gt;</span>
          This is a very long text that gets cut off but has a helpful tooltip
        <span>&lt;/</span><span>p</span><span>&gt;</span>
      <span>&lt;/</span><span>TooltipTrigger</span><span>&gt;</span>
      <span>&lt;</span><span>TooltipContent</span><span>&gt;</span>
        <span>&lt;</span><span>p</span><span>&gt;</span>This is a very long text that gets cut off but has a helpful tooltip<span>&lt;/</span><span>p</span><span>&gt;</span>
      <span>&lt;/</span><span>TooltipContent</span><span>&gt;</span>
    <span>&lt;/</span><span>Tooltip</span><span>&gt;</span>
  <span>&lt;/</span><span>TooltipProvider</span><span>&gt;</span>
<span>// Disabled button tooltip example</span>
<span>&lt;</span><span>button</span> <span>disabled</span> <span>title</span><span>=</span><span>"You need to complete the form before submitting"</span><span>&gt;</span>
  Submit
<span>&lt;/</span><span>button</span><span>&gt;</span>
```

Adding these tooltips makes your app feel more thoughtful and polished. Small touches like this show you care about the user’s experience.

[![Tip #7](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhhdftzz3gzt25dlvbps4.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhhdftzz3gzt25dlvbps4.png)

___

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#8-use-contextual-colors-for-feedback)8\. Use Contextual Colors for Feedback

Colors play a crucial role in guiding users through your app. Using the right color for the right situation makes the interface more intuitive and easier to understand. For example, reddish tones for errors, greenish for success, yellowish for warnings, and blueish for informational messages.

### [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#pro-tip)Pro Tip:

Stick to established color conventions to avoid confusing your users. Most people recognize red as an error or danger indicator and green as a success marker, so leverage that!  

```
<span>// Example of contextual colors in action</span>
              <span>&lt;</span><span>div</span> <span>className</span><span>=</span><span>"flex items-center rounded-md border border-red-200 bg-red-100/75 p-3"</span><span>&gt;</span>
                <span>&lt;</span><span>AlertCircle</span> <span>className</span><span>=</span><span>"mr-2 text-red-700"</span> <span>/&gt;</span>
                <span>&lt;</span><span>p</span> <span>className</span><span>=</span><span>"text-red-800"</span><span>&gt;</span>Error: Something went wrong!<span>&lt;/</span><span>p</span><span>&gt;</span>
              <span>&lt;/</span><span>div</span><span>&gt;</span>
              <span>&lt;</span><span>div</span> <span>className</span><span>=</span><span>"flex items-center rounded-md border border-green-400 bg-green-100/75 p-3"</span><span>&gt;</span>
                <span>&lt;</span><span>CheckCircle</span> <span>className</span><span>=</span><span>"mr-2 text-green-700"</span> <span>/&gt;</span>
                <span>&lt;</span><span>p</span> <span>className</span><span>=</span><span>"text-green-800"</span><span>&gt;</span>Success: Operation completed!<span>&lt;/</span><span>p</span><span>&gt;</span>
              <span>&lt;/</span><span>div</span><span>&gt;</span>
```

By following these color conventions, you make it much easier for users to quickly understand what's happening in your app without over-explaining.

This small detail not only helps in making your UI more intuitive but also enhances the visual hierarchy by linking colors with actions and outcomes.

[![Tip #8](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fuac58234i1tb64s2nql1.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fuac58234i1tb64s2nql1.png)

___

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#9-icons-should-enhance-not-distract)9\. Icons Should Enhance, Not Distract

Don’t overload your UI with icons for the sake of it. Every icon should have a clear purpose and enhance the user experience.

### [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#pro-tip)Pro Tip:

Keep icons simple, and recognizable, and ensure they match the action they represent.  

```
<span>&lt;</span><span>Icon</span> <span>name</span><span>=</span><span>"trash"</span> <span>onClick</span><span>=</span><span>{</span><span>deleteItem</span><span>}</span> <span>/&gt;</span>
<span>&lt;</span><span>Icon</span> <span>name</span><span>=</span><span>"edit"</span> <span>onClick</span><span>=</span><span>{</span><span>editItem</span><span>}</span> <span>/&gt;</span>
```

Use icons to clarify actions, not clutter your UI.

___

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#10-dont-reinvent-the-wheel-use-solid-ui-libraries)10\. Don’t Reinvent the Wheel — Use Solid UI Libraries

It’s easy to fall into the trap of thinking you have to code _everything_ from scratch. But guess what? You don’t! There are fantastic UI libraries out there like Ant Design, Shadcn UI, Material UI, and Chakra UI that can save you a ton of time and effort. These libraries provide well-tested, accessible, and consistent components, so you can focus on what makes your app unique instead of redoing the basics.

### [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#pro-tip)Pro Tip:

If you’re trying to move fast or you’re working on a project with tight deadlines, leverage these libraries. But if you’re building something purely for learning, feel free to dive into the details and build your own components.  

```
<span>import</span> <span>{</span> <span>Button</span> <span>}</span> <span>from</span> <span>'</span><span>antd</span><span>'</span><span>;</span>

<span>function</span> <span>MyApp</span><span>()</span> <span>{</span>
  <span>return</span> <span>&lt;</span><span>Button</span> <span>type</span><span>=</span><span>"primary"</span><span>&gt;</span>Click Me<span>&lt;/</span><span>Button</span><span>&gt;;</span>
<span>}</span>
```

[![Tip #10](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fstdp6j3ymy1hz7b7wgc6.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fstdp6j3ymy1hz7b7wgc6.png)

Using these libraries helps ensure that your app looks polished, functions well, and remains consistent, all while saving you time. You can always customize things later, but starting with a solid foundation helps you move faster.

No need to code a button from scratch when there’s already a beautifully styled, responsive one you can drop right in!

## [](https://dev.to/harimanok/make-your-app-feel-better-11-ui-tips-for-beginnermid-level-developers-1e1n?context=digest#conclusion)Conclusion

The key to a great UI is attention to detail. These small tweaks—consistent spacing, clear hierarchy, thoughtful empty/loading/error states, and balanced use of whitespace—make a world of difference. Whether you’re working on your first app or your 50th, take the time to polish these areas, and your users will thank you.

Got any thoughts or questions? Let me know, I’m happy to expand on these topics or chat more. Happy coding, and may your apps always feel amazing! ✨
