---
created: 2024-09-30T10:27:35 (UTC -03:00)
tags: []
source: https://blog.logrocket.com/developing-modals-using-only-css-popover-api/
author: Daniel Schwarz
---

# Developing modals using only CSS and the Popover API - LogRocket Blog

> ## Excerpt
> The Dialog and Popover approach to modals requires less code and and fewer files than using JavaScript method, making it less error-prone.

---
![](http://blog.logrocket.com/wp-content/uploads/2023/04/logrocket-logo-1.png)

## See how LogRocket's AI-powered error tracking works

### no signup required

Check it out

Modals once had a bad reputation because they were so complicated to build from scratch. They were often buggy and had terrible usability, not to mention the many accessibility requirements that had to be met. To address these issues, about a decade ago, the `<dialog>` element was introduced, along with supporting JavaScript methods and CSS properties. But, what if we could take it a step further by eliminating the need for JavaScript and using the new Popover API instead?

![Developing Modals Using Only CSS And The Popover API](https://blog.logrocket.com/wp-content/uploads/2024/09/developing-modals-using-only-css-popover-api.png)

In this article, you’ll learn how to combine the Dialog API with the Popover API and a bit of CSS to create modals without JavaScript.

## Why develop modals without JavaScript?

The Dialog and Popover approach asks for less code, fewer languages, and fewer files, so it’s much more manageable and less error-prone than using JavaScript.

Additionally, Node.js, frameworks, browser extensions, and even other snippets of JavaScript can interfere with a JavaScript approach to modals, potentially causing errors. To add to this, you often have to choose between deferring JavaScript or letting it block the rendering of the page.

## Why `<dialog>` and Popover? Aren’t they different things?

Popovers are non-modal, meaning that users will still be able to interact with what’s underneath the [top layer](https://developer.mozilla.org/en-US/docs/Glossary/Top_layer). This also means that the [tab order](https://www.a11y-collective.com/glossary/tab-order/) won’t be contained to the popover. On the other hand, dialogs are modal if implemented correctly, meaning that users won’t be able to interact with the “bottom” layers. In terms of tab order, the focus will cycle through the focusable elements of the `<dialog>` and browser UI only. To me, this is the key factor that determines whether I use a dialog or popover.

Something else to consider is the fact that dialogs are invoked using JavaScript, whereas popovers are fully HTML-based. But what if we want to develop a modal without JavaScript?

Well, I’ve discovered that you can use Dialog and Popover, as Dialog is an element and Popover is an attribute. This means that it’s possible to develop modals using just HTML (and CSS of course), and that’s exactly what you’ll learn how to do in this tutorial. I’ll also show you how to style the backdrop (`::backdrop`) and even prevent scrolling using only CSS.

## Step 1: Coding the modal without Popover (for comparison)

To understand how this approach works, we’ll code the modal using the `<dialog>` element, which has been supported by web browsers for over a decade now, and JavaScript. We’ll then replace the JavaScript with the Popover API as well as some CSS so that you can see firsthand how much shorter the code is.

Let’s start with the `<dialog>` element:

```html
<dialog></dialog>
```

```html
<dialog id="dialogA"></dialog>
```

```html
<dialog id="dialogA">
    <button class="closeDialog">Close dialogA</button>
</dialog>
```

```html
<button data-dialog="dialogA">Open dialogA</button>
```

What’s happening here is that we’re selecting all elements with the `data-dialog` attribute and attaching a click event listener to each one — this enables us to have as many modals as we want.

Whenever the user clicks on one of those buttons, we read the value of `data-dialog`, select the modal that it corresponds to by matching the `id`, and then store the node in a variable called `dialog`. After that, we call the `.showModal()` method on the `dialog` node. If you opt to show the `<dialog>` element in any other way, it will not be a modal dialog as the background will not be inert, which is fine if you don’t want a modal. However, the Popover API would be a better option in this case.

The next part of the code renders the document unscrollable by setting the CSS `overflow` property to `hidden` on the `<html>` and `<body>`. After that, we have another click event listener that closes the `<dialog>` using the `.close()` method and removes the `overflow` property whenever the close button (`.closeDialog`) is clicked:

```javascript
/* Select and then loop all elements with the data-dialog custom HTML attribute */
document.querySelectorAll("[data-dialog]").forEach(button => {

    /* Make each one listen for a click */
    button.addEventListener("click", () => {
    
        /* Match the value of data-dialog to the dialog with the same id value */
        const dialog = document.querySelector(`#${ button.dataset.dialog }`);
    
        /* Show it! */
        dialog.showModal();
        
        /* Prevent scrolling */
        document.body.style.overflow = "hidden";
        document.documentElement.style.overflow = "hidden";
        
        /* Listen for a click on the dialog's close button */dialog.querySelector(".closeDialog").addEventListener("click", () => {
        
            /* Close the dialog */
            dialog.close();
            
            /* Re-enable scrolling */
            document.body.style.removeProperty("overflow");
            document.documentElement.style.removeProperty("overflow");
        
        });
    
    });

});
```

Well, there are now, and that’s what we’re going to look at next.

First, let’s just get the CSS out of the way. The following CSS one-liner prevents the document (the `<body>`, or what’s behind the popover) from being scrollable while it’s open. We’ve restricted this behavior to just dialogs for now:

```css
body:has(dialog:popover-open) { overflow: hidden; }
```

```css
/* This one is more semantic */
body:has(dialog:modal) { overflow: hidden; }

/* However, this one works just fine */
body:has(dialog[open]) { overflow: hidden; }
```

```javascript
document.querySelectorAll("[data-dialog]").forEach(button => {

    button.addEventListener("click", () => {
    
        const dialog = document.querySelector(`#${ button.dataset.dialog }`);
    
        dialog.showModal();
        dialog.querySelector(".closeDialog").addEventListener("click", () => dialog.close());
    
    });

});
```

Now, I’ll demonstrate how to eliminate the remaining JavaScript using the new Popover API. First, swap the `data-dialog` custom HTML attribute for the `popovertarget` attribute. There’s no need to change the value:

```html
<button popovertarget="dialogA">Open dialogA</button>
```

```html
<dialog id="dialogA">
    <button popovertarget="dialogA">Close dialogA</button>
</dialog>
```

```javascript
<dialog id="dialogA" popover>
    <button popovertarget="dialogA">Close dialogA</button>
</dialog>
```

However, popovers aren’t modal, remember? So we need to fix that.

## Step 4: Making popovers modal

Popovers aren’t modal and neither are dialogs unless you specifically open and close them with the `.showModal()` and `.close()` JavaScript methods.

This means that while we currently have a semantic HTML element (`<dialog>`) (which is nice to have, I suppose, but in practice has no benefit at this current time) as well as the Popover API handling the ultra-lightweight implementation for the developer’s benefit, but we don’t have the accessibility benefits for the user. For example, the fact that our popovers aren’t modal means that the focus isn’t trapped to the dialog, so those interacting using a keyboard or assistive technology can accidentally fall out of the dialog and into the potentially blacked-out document.

W3C specifies that if a modal is visible, then the document must be inert. Here’s what that means:

-   The `<body>` must have the `aria-hidden="true"` attribute and value so that assistive technologies are aware that a modal is open
-   The `<body>` must have the `pointer-events: none` CSS property and value so that interactables cannot be interacted with
-   The `<body>` must have the `user-select: none` CSS property and value so that text cannot be selected
-   Editable elements (e.g., those with the `contenteditable` attribute whose value evaluates to something ‘truthy’) must be rendered uneditable
-   The web browser’s find-in-page feature should not be able to find anything in the `<body>`
-   Links must have the `tabindex: -1` attribute and value so that they cannot be focused upon

A couple of those are definitely possible and we already have the CSS rule set up to implement them:

```css
body:has(dialog:popover-open) {
    overflow: hidden;
    user-select: none;
    pointer-events: none;
}
```

```css
body:has(dialog:popover-open) {

    overflow: hidden;
    
    main {
        opacity: 0; /* Doesn't render the body inert */
        display: none; /* Does, but causes content shift */
        visibility: hidden; /* Does! */
    }
    
}
```

However, if you prefer a translucent background and perhaps even a filter that blurs the background, what you’re looking for is the `inert` HTML attribute, which provides all of the accessibility needed in a single attribute. The `.showModal()` JavaScript method accomplishes the same thing. In this case, adding just a small JavaScript one-liner wouldn’t be so bad.

___

### More great articles from LogRocket:

-   Don't miss a moment with [The Replay](https://lp.logrocket.com/subscribe-thereplay), a curated newsletter from LogRocket
-   [Learn](https://blog.logrocket.com/rethinking-error-tracking-product-analytics/) how LogRocket's Galileo cuts through the noise to proactively resolve issues in your app
-   Use React's useEffect [to optimize your application's performance](https://blog.logrocket.com/understanding-react-useeffect-cleanup-function/)
-   Switch between [multiple versions of Node](https://blog.logrocket.com/switching-between-node-versions-during-development/)
-   [Discover](https://blog.logrocket.com/using-react-children-prop-with-typescript/) how to use the React children prop with TypeScript
-   [Explore](https://blog.logrocket.com/creating-custom-mouse-cursor-css/) creating a custom mouse cursor with CSS
-   Advisory boards aren’t just for executives. [Join LogRocket’s Content Advisory Board.](https://lp.logrocket.com/blg/content-advisory-board-signup) You’ll help inform the type of content we create and get access to exclusive meetups, social accreditation, and swag.

___

The JavaScript examples below (external and internal, depending on your preferences) demonstrate how to listen to your popover and toggle the `inert` attribute every time the popover itself is toggled — and that’s it!

```javascript
/* External JavaScript */
document.querySelectorAll("dialog[popover]").forEach(dialog => dialog.addEventListener("toggle", () => document.body.toggleAttribute("inert")));
```

## How to style the modal’s backdrop

I’ve mentioned backdrops a few times, so here’s the deal regarding backdrops (`::backdrop`) and popovers. Anyone who has used `<dialog>` before will know that they only get access to backdrops when you utilize the `.showModal()` JavaScript method, as is the case with many modal features, which we’ve seen in this article’s demonstrations.

What’s so great about popovers isn’t just that they can utilize backdrops like dialogs can, but that they don’t require JavaScript like dialogs do. For that reason, we can implement a backdrop on our modal with no problem. The example below adds a translucent black background with a blurry filter:

```css
dialog:popover-open {
    filter: blur(5px);
    background: hsl(0 0 0 / 90%);
}
```

## Closing the modal without the close button

If you provide the `popover` attribute with the `manual` value (so `<dialog id="dialogA" popover="manual">`), you can prevent the modal from being closable by clicking on the backdrop. Great — in most cases that’s preferable to me, as I often accidentally close modals and lose my progress.

The problem with this is that it also prevents the modal from being closable by pressing the `esc` key, a notable feature for people that operate websites using their keyboard. The best way to handle this is to forgo the `manual` value and just ensure that progress isn’t lost when the modal gets closed (luckily, this is the default behavior anyway).

## Conclusion

So there you have it — HTML/CSS-only accessible modals without JavaScript (or with just a tiny bit of JavaScript depending on what you’re trying to achieve aesthetically). JavaScript-free modals have many benefits — the code’s smaller, more robust, easier to manage, and it doesn’t block rendering. Plus, if you enjoy implementing web browser-supported cutting-edge features, then the [Popover API](https://developer.mozilla.org/en-US/docs/Web/API/Popover_API) is definitely worth exploring.

Got a question? Drop it in the comment section below, and thanks for reading!

## Is your frontend hogging your users' CPU?

As web frontends get increasingly complex, resource-greedy features demand more and more from the browser. If you’re interested in monitoring and tracking client-side CPU usage, memory usage, and more for all of your users in production, [try LogRocket](https://lp.logrocket.com/blg/css-signup).

[![LogRocket Dashboard Free Trial Banner](https://blog.logrocket.com/wp-content/uploads/2019/12/cpu-memory-usage.png)](https://lp.logrocket.com/blg/css-signup)

[LogRocket](https://lp.logrocket.com/blg/css-signup) is like a DVR for web and mobile apps, recording everything that happens in your web app, mobile app, or website. Instead of guessing why problems happen, you can aggregate and report on key frontend performance metrics, replay user sessions along with application state, log network requests, and automatically surface all errors.

Modernize how you debug web and mobile apps — [start monitoring for free](https://lp.logrocket.com/blg/css-signup).
