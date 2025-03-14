---
created: 2024-09-26T10:45:26 (UTC -03:00)
tags: []
source: https://blog.openreplay.com/zoneless-change-detection-in-angular-18/
author: Ugochukwu Sabastine
     Sep 26, 2024
        · 4 min read
---

# Zoneless Change Detection in Angular 18

> ## Excerpt
> A new feature in Angular you should know

---
> Change detection is important in [Angular](https://angular.dev/), ensuring that the UI shows the application’s current state. Traditionally, Angular uses [Zone.js](https://angular.dev/best-practices/zone-pollution) to automatically detect and apply these changes to the UI, but with the Angular 18 release, a new approach was introduced—zoneless change detection, as this article will show.

Change detection in Angular involves tracking and updating changes in data components to the UI. It ensures that interaction is displayed in real-time when we interact with an Angular app. This article will take us through change detection, how `Zone.js` works, and the reason for the shift to applications without zones. We will also see how to set up a change detection process in a Zoneless application.

`Zone.js` works by [monkey-patching](https://blog.openreplay.com/monkey-patching-in-javascript/) all `async` APIs and creating a zone for each `async` task. It will keep track of operations from their beginning to their end. The change detection mechanism is triggered when the task is done, and the UI is updated.

It’s important to note that Angular will not discontinue `Zone.js` since many apps have already been made using it. They will continue to support `Zone.js` by making critical fixes and security patches.

## Why is Angular moving towards Zoneless?

As applications scale, the hands-off reactivity used by `Zone.js` begins to show some weaknesses. Some of these challenges are:

Maintenance and performance safeguarding: Maintaining reactivity becomes more challenging as the application grows. This is because Zone.js relies on DOM events and `async` tasks to detect changes in the application state, but it doesn’t know whether the state really changed. This causes it to trigger synchronization more often than it is supposed to.

Debugging reactivity: It is difficult to identify the root cause of reactivity problems. With Zone.js, it is challenging to know if the code breaks because it is outside the zone. This is a result of Zone.js\` challenging stack trace interpretation.

-   Cost of Zones with new Web APIs: Application loading cost and start-up time increase when new Web APIs are added. This will increase the application bundle size. Removing this dependency makes the application more efficient.

## Introduction to Zoneless Applications

Zoneless applications do not use Zone.js for change detection. Rather, the component itself notifies Angular’s change detection mechanism to update the UI after the completion of an `async` function.

It gives developers additional flexibility over the timing and mechanism of change detection, improving the application’s performance and efficiency.

We will demonstrate with some examples without `Zone.js` that require change detection. To follow up on this tutorial, first install Angular 18 by running the command below.

```
<span><span>npm install -g @angular/cli</span></span>
```

Run the command below in the command line to create a new Angular project.

```
<span><span>ng new </span><span>&lt;</span><span>filename</span><span>&gt;</span></span>
```

Open the file in your code editor. After that, disable `Zone.js`. We can do this by removing `Zone.js` from the `polyfills` in the `angular.json` file.

```
<span><span>"polyfills"</span><span>: [</span></span>
<span><span>  </span><span>"zone.js"</span><span> </span><span>// remove zone.js</span></span>
<span><span>]</span></span>
```

Then in the `app.config.ts` file, add `provideExperimentalZonelessChangeDetection()` in the `providers` array. After you’ve done it, you have successfully configured your Angular app to use the Zoneless feature for change detection instead of `Zone.js`.

```
<span><span>export</span><span> </span><span>const</span><span> </span><span>appConfig</span><span>:</span><span> </span><span>ApplicationConfig</span><span> </span><span>=</span><span> {</span></span>
<span><span>  providers: [</span></span>
<span><span>    </span><span>provideExperimentalZonelessChangeDetection</span><span>(),</span></span>
<span><span>    </span><span>provideRouter</span><span>(routes),</span></span>
<span><span> ],</span></span>
<span><span>};</span></span>
```

### Setting output in the component

After configuring our application to be Zoneless, we will test the change detection mechanism by defining a variable and updating its state.

In the code below, we update the `PageNumber` to 2 using the [ngOnInit](https://angular.dev/guide/components/lifecycle#ngoninit) method. Angular’s change detection mechanism detects this change and automatically updates the UI.

```
<span><span>import</span><span> { RouterOutlet } </span><span>from</span><span> </span><span>'@angular/router'</span><span>;</span></span>
<span><span>import</span><span> { CommonModule } </span><span>from</span><span> </span><span>'@angular/common'</span><span>;</span></span>
<span><span>import</span><span> { Component, OnInit } </span><span>from</span><span> </span><span>'@angular/core'</span><span>;</span></span>
<span></span>
<span><span>@</span><span>Component</span><span>({</span></span>
<span><span>  selector: </span><span>'app-root'</span><span>,</span></span>
<span><span>  standalone: </span><span>true</span><span>,</span></span>
<span><span>  imports: [RouterOutlet, CommonModule],</span></span>
<span><span>  template: </span><span>`</span></span>
<span><span> &lt;div&gt;Page Number: &#123;&#123; PageNumber &#124;&#124;&lt;/div&gt;</span></span>
<span><span> `</span><span>,</span></span>
<span><span>})</span></span>
<span><span>export</span><span> </span><span>class</span><span> </span><span>AppComponent</span><span> </span><span>implements</span><span> </span><span>OnInit</span><span> {</span></span>
<span><span>  </span><span>title</span><span> </span><span>=</span><span> </span><span>'zoneless'</span><span>;</span></span>
<span><span>  </span><span>PageNumber</span><span> </span><span>=</span><span> </span><span>0</span><span>;</span></span>
<span></span>
<span><span>  </span><span>ngOnInit</span><span>()</span><span>:</span><span> </span><span>void</span><span> {</span></span>
<span><span>    </span><span>this</span><span>.PageNumber </span><span>+=</span><span> </span><span>2</span><span>;</span></span>
<span><span> }</span></span>
<span><span>}</span></span>
```

In the image below, the `PageNumber` variable is automatically updated from 0 to 2.

![setting](https://blog.openreplay.com/images/zoneless-change-detection-in-angular-18/images/image1.png)

### Timer event change

Timer event change in Zoneless isn’t automatically updated. It works like with [OnPush](https://angular.dev/best-practices/skipping-subtrees#using-onpush) as you’ll need to manually trigger it because there’s no automatic change detection as in `Zone.js`.

To test this, let’s create a timer event using `setTimeout()` to update the `PageNumber` to 2 after 1 second.

```
<span><span>@</span><span>Component</span><span>({</span></span>
<span><span>  template: </span><span>`</span></span>
<span><span> &lt;div&gt;Page Number: &#123;&#123; PageNumber &#124;&#124;&lt;/div&gt;</span></span>
<span><span> `</span><span>,</span></span>
<span><span>})</span></span>
<span><span>export</span><span> </span><span>class</span><span> </span><span>AppComponent</span><span> </span><span>implements</span><span> </span><span>OnInit</span><span> {</span></span>
<span><span>  </span><span>title</span><span> </span><span>=</span><span> </span><span>'zoneless'</span><span>;</span></span>
<span><span>  </span><span>PageNumber</span><span> </span><span>=</span><span> </span><span>0</span><span>;</span></span>
<span></span>
<span><span>  </span><span>ngOnInit</span><span>()</span><span>:</span><span> </span><span>void</span><span> {</span></span>
<span><span>    </span><span>setTimeout</span><span>(() </span><span>=&gt;</span><span> {</span></span>
<span><span>      </span><span>this</span><span>.PageNumber </span><span>+=</span><span> </span><span>2</span><span>;</span></span>
<span><span> }, </span><span>1000</span><span>);</span></span>
<span><span> }</span></span>
<span><span>}</span></span>
```

The image shows that the `PageNumber` variable is not set to 2 after 1 second.

![image](https://blog.openreplay.com/images/zoneless-change-detection-in-angular-18/images/image2.png)

This shows that Angular might not automatically detect asynchronous operations. To initialize it manually, [ChangeDetectorRef](https://angular.dev/api/core/ChangeDetectorRef) can be applied to trigger the event handler.

To do this, inject `ChangeDetectorRef` into the `AppComponent` and use `changeDetectorRef.detectChanges()` function to trigger the update.

```
<span><span>export</span><span> </span><span>class</span><span> </span><span>AppComponent</span><span> </span><span>implements</span><span> </span><span>OnInit</span><span> {</span></span>
<span><span>  </span><span>title</span><span> </span><span>=</span><span> </span><span>'zoneless'</span><span>;</span></span>
<span><span>  </span><span>changeDetectorRef</span><span> </span><span>=</span><span> </span><span>inject</span><span>(ChangeDetectorRef);</span></span>
<span><span>  </span><span>PageNumber</span><span> </span><span>=</span><span> </span><span>0</span><span>;</span></span>
<span></span>
<span><span>  </span><span>ngOnInit</span><span>()</span><span>:</span><span> </span><span>void</span><span> {</span></span>
<span><span>    </span><span>setTimeout</span><span>(() </span><span>=&gt;</span><span> {</span></span>
<span><span>      </span><span>this</span><span>.PageNumber </span><span>+=</span><span> </span><span>2</span><span>;</span></span>
<span><span>      </span><span>this</span><span>.changeDetectorRef.</span><span>detectChanges</span><span>();</span></span>
<span><span> }, </span><span>1000</span><span>);</span></span>
<span><span> }</span></span>
<span><span>}</span></span>
```

The image shows that the `PageNumber` variable is updated to 2 after 1 second.

![signal update](https://blog.openreplay.com/images/zoneless-change-detection-in-angular-18/images/image3.gif)

### Signal Update

In Zoneless applications, the [signal](https://angular.dev/guide/signals#) API automatically updates the reactive state to the UI, just as it does in Zone.js.

To test this, let’s create a `signal` and initialize it with the value 5. Then update this value by calling the `this.signal.set()` after 1s delay.

```
<span><span>@</span><span>Component</span><span>({</span></span>
<span><span>  template: </span><span>`</span></span>
<span><span> &lt;div&gt;Signal: &#123;&#123; signal() &#124;&#124;&lt;/div&gt;</span></span>
<span><span> `</span><span>,</span></span>
<span><span>})</span></span>
<span><span>export</span><span> </span><span>class</span><span> </span><span>AppComponent</span><span> </span><span>implements</span><span> </span><span>OnInit</span><span> {</span></span>
<span><span>  </span><span>title</span><span> </span><span>=</span><span> </span><span>'zoneless'</span><span>;</span></span>
<span><span>  </span><span>signal</span><span> </span><span>=</span><span> </span><span>signal</span><span>(</span><span>5</span><span>);</span></span>
<span></span>
<span><span>  </span><span>ngOnInit</span><span>()</span><span>:</span><span> </span><span>void</span><span> {</span></span>
<span><span>    </span><span>setTimeout</span><span>(() </span><span>=&gt;</span><span> {</span></span>
<span><span>      </span><span>this</span><span>.signal.</span><span>set</span><span>(</span><span>2</span><span>);</span></span>
<span><span> }, </span><span>1000</span><span>);</span></span>
<span><span> }</span></span>
<span><span>}</span></span>
```

The image shows that the `signal` is updated from 5 to 2 in the Zoneless application.

![async pipe (1)](https://blog.openreplay.com/images/zoneless-change-detection-in-angular-18/images/image4.gif)

### Event Change

The Angular detection mechanism detects when a user interacts with elements in the case below a button and triggers an update.

Let’s create a property `eventChange` and set it to 0. In the template, we will define a button that, on clicking, will change the value of the event to 200.

```
<span><span>@</span><span>Component</span><span>({</span></span>
<span><span>  template: </span><span>`</span></span>
<span><span> &lt;button (click)="eventChange = 200"&gt;Click here&lt;/button&gt; &#123;&#123; eventChange &#124;&#124;</span></span>
<span><span> `</span><span>,</span></span>
<span><span>})</span></span>
<span></span>
<span><span>export</span><span> </span><span>class</span><span> </span><span>AppComponent</span><span> {</span></span>
<span><span>  </span><span>title</span><span> </span><span>=</span><span> </span><span>'zoneless'</span><span>;</span></span>
<span><span>  </span><span>eventChange</span><span> </span><span>=</span><span> </span><span>0</span><span>;</span></span>
<span><span>}</span></span>
```

In the image below, once the button is clicked the `eventChange` variable is automatically updated to 200.

![event change](https://blog.openreplay.com/images/zoneless-change-detection-in-angular-18/images/image5.gif)

### Async pipe

The `async pipe` subscribes to an `Observable` and returns the latest value emitted. When this new value is emitted, the `async pipe` automatically updates the UI after triggering the change detection.

In the code below, we create an `Observable` `time$` that emits a value every second. The `async pipe` updates the displayed value, which subscribes to `time$`.

```
<span><span>@</span><span>Component</span><span>({</span></span>
<span><span>  template: </span><span>`</span></span>
<span><span> &lt;div&gt;Current Time: &#123;&#123; time$ | async &#124;&#124;&lt;/div&gt;</span></span>
<span><span> `</span><span>,</span></span>
<span><span>})</span></span>
<span><span>export</span><span> </span><span>class</span><span> </span><span>AppComponent</span><span> {</span></span>
<span><span>  </span><span>time$</span><span> </span><span>=</span><span> </span><span>interval</span><span>(</span><span>1000</span><span>); </span><span>// Emits values every second</span></span>
<span><span>}</span></span>
```

The image below shows the value of `time$`, updated every second.

![async pipe](https://blog.openreplay.com/images/zoneless-change-detection-in-angular-18/images/image6.gif)

It is vital to note that Zoneless change detection is still an experimental feature and major updates could be added to its API.

## Conclusion

Angular moving towards Zoneless change detection will address some challenges with `Zone.js` such as maintenance, performance, and debugging thereby making Angular applications perform more efficiently. We looked through examples where we needed to manually trigger updates using `ChangeDetectorRef`, and leverage `async pipe` for automatic updates. Zoneless change detection is still experimental but offers a promising alternative for building more scalable applications.

## Additional Resources

-   [Zoneless application](https://angular.dev/guide/experimental/zoneless)

### Gain Debugging Superpowers

Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with [OpenReplay](https://openreplay.com/) — the most advanced open-source session replay tool for developers. Check our [GitHub repo](https://github.com/openreplay/openreplay) and join the thousands of developers in our community.

[![OpenReplay](https://blog.openreplay.com/media/openreplay-git-hero.svg)](https://github.com/openreplay/openreplay)
