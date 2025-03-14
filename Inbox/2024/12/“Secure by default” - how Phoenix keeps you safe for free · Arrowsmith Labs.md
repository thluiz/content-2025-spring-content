---
created: 2024-12-04T12:09:41 (UTC -03:00)
tags: []
source: https://arrowsmithlabs.com/blog/secure-by-default-how-phoenix-keeps-you-safe-for-free?utm_medium=email&utm_source=elixir-radar
author: 
---

# “Secure by default” - how Phoenix keeps you safe for free · Arrowsmith Labs

> ## Excerpt
> Discover how Phoenix's "secure by default" framework keeps your applications safe without extra cost or effort. Learn about key security features like XSS, CSRF protection, secure browser headers, and SQL injection prevention, automatically integrated for your peace of mind. Perfect for developers at all levels, Phoenix ensures robust security from day one, allowing you to focus on innovation. Whether you're a beginner or seasoned pro, safeguard your projects effortlessly with Phoenix while educating yourself on vital security practices. Save time and reduce risk with Phoenix’s built-in security advantages.

---
![](https://arrowsmithlabs.com/images/blog/secure_by_default-ad494932bd71b88b313926fede39e8bf.jpg?vsn=d)

_Learn Phoenix LiveView_ is the comprehensive tutorial that teaches you everything you need to build a complex, realistic, fully-featured web app with Phoenix LiveView. [Click here to learn more](https://phoenixliveview.com/?ref=blog)!

I recently made the mistake of getting embroiled in a Twitter<sup x-tooltip="tooltip">[1]</sup> drama. A popular Javascript “starter kit” framework was found to be riddled with embarrassingly basic security flaws, and I joined the resulting pile-on with some threads that went quite viral. I may have made some enemies.

I’m not going to relitigate the argument about that one particular framework - I don’t care. Instead I want to make a more general point, because I got a lot of replies that, frankly, disturbed me.

A lot of “indie hackers” apparently think that security isn’t important. Sure, it’s better to be secure than insecure, but you have a business to launch - security is a cost you can live without, so get your MVP out the door asap then you can worry about security later. Right? [People actually think like this](https://x.com/dagorenouf/status/1835610714318221326).

To me this is like a car manufacturer saying “just make it driveable, we’ll add safety later”. It’s more than reckless; it speaks to something fundamentally wrong about how you approach engineering. Safety and security aren’t something you slap on later as an afterthought. They should be woven into your design from day one - and they don’t have to cost you anything!

A well-designed technology is **secure by default**. You shouldn’t have to spend much effort if any to keep your app secure. Just use secure tooling, and most of the work is done for you. If anything a secure app should take _less_ time to build than an insecure one, because you’re simply working with the framework’s defaults, not against them.

We were all beginners once. We can’t expect every developer to know about and protect against every possible security issue. A healthy software industry is one where we make things as easy as possible for juniors as well as seniors; let’s use tooling that automates the “solved problems” so we can focus on the interesting stuff that hasn’t been done already a million times before.

To see what I mean, let’s explore some of the ways in which Phoenix keeps you secure by default. None of this is specific to Phoenix - everything I list below is also a feature of Rails and many other frameworks too (although, apparently, not all of them.)

Want more posts like this in your inbox?

No spam. Unsubscribe any time.

## XSS

Phoenix automatically HTML-escapes any text that you output between `<%=` and `%>`. So if a user tries to maliciously insert a `<script>` tag:

![](https://arrowsmithlabs.com/images/blog/secure_by_default/xss_0.png)

…then it’ll be rendered harmlessly as text, rather than parsed as valid HTML:

![](https://arrowsmithlabs.com/images/blog/secure_by_default/xss_1.png)

This protects you against [Cross-Site Scripting](https://owasp.org/www-community/attacks/xss/) (XSS) attacks.

## CSRF

Phoenix automatically applies CSRF protection to all non-GET endpoints. Submit a request without a valid CSRF token and you’ll get an error:

![](https://arrowsmithlabs.com/images/blog/secure_by_default/csrf.png)

You don’t normally need to think about this. Phoenix’s `<.form>` component outputs a hidden input containing the `_csrf_token`, so the CSRF check will pass and the form will submit successfully.

## Secure browser headers

The default Phoenix router calls the `put_secure_browser_headers` plug in its `:browser` pipeline. This sets some headers on the HTTP response that enhance security, as detailed in [the docs](https://hexdocs.pm/phoenix/Phoenix.Controller.html#put_secure_browser_headers/2):

![](https://arrowsmithlabs.com/images/blog/secure_by_default/secure_headers.png)

## SQL Injection

Ecto, Phoenix’s data mapping library, automatically parameterizes its inputs, protecting you against [SQL injection](https://owasp.org/www-community/attacks/SQL_Injection) attacks:

![](https://arrowsmithlabs.com/images/blog/secure_by_default/sql_injection.jpg)

If only [Little Bobby Tables](https://xkcd.com/327/)’s school had used Ecto, they would have been safe.

## Auth

Practically every web app needs an authentication system by which users can create accounts and log in. There’s no excuse in `<%= current_year %>` for getting this wrong. Login systems are as old as the web and you don’t need to innovate. You can just use someone else’s battle-tested tooling.

Phoenix makes it super easy: just run [`mix phx.gen.auth`](https://hexdocs.pm/phoenix/mix_phx_gen_auth.html) and you get user registrations, login, email confirmation and password resets, for only 2 seconds’ work:

![](https://arrowsmithlabs.com/images/blog/secure_by_default/mix_phx_gen_auth.png)

This contains everything you need to keep your login system secure, including some less well-known details, such as protecting against [timing attacks](https://ropesec.com/articles/timing-attacks/):

![](https://arrowsmithlabs.com/images/blog/secure_by_default/timing.png)

(For more on how this login system works and what makes it secure, see [this thread I wrote](https://x.com/ThatArrowsmith/status/1850842089103188181).)

___

These attacks and their mitigations are all worth knowing about. But my point is that you don’t _need_ to know about any of them. Phoenix gives you all this security for free - even if you’re a total beginner who’s never heard of XSS, CSRF, SQL injection etc., you’re still protected.

You can bypass these protections, but you have to do so explicitly. For example, you can render unescaped HTML (bypassing XSS protection) using [Phoenix.HTML.raw/1](https://hexdocs.pm/phoenix_html/Phoenix.HTML.html#raw/1). So the option is there if you need it - but it’s not the kind of thing you might do accidentally.

This protects us against ourselves. Security failures often result from ignorance: we don’t guard against a particular type of attack, e.g. XSS, because we didn’t know we needed to. Or they can result from sloppiness: everyone makes mistakes, and we only need to be wrong once for an attacker to get his shot. In any case, using secure-by-default tooling means that our software is secure _even when we’re ignorant and sloppy_.

Even better, secure-by-default tooling _teaches_ you about security. Use Phoenix (or Rails) for long enough and eventually you’ll bump into its safety barriers. This is a learning opportunity!

For example, I don’t remember how I originally learned about XSS, but it probably involved me trying to render some HTML in Rails and noticing that the “<”s and “>”s were being changed to the escaped versions. Then I looked it up, and learned something new about web security.

This is far better than the alternative - learning about XSS because your tooling _didn’t_ protect you, and now you’ve accidentally shipped something insecure and a hacker has ruined you.

This shouldn’t be difficult, but recent events suggest otherwise. As I said on Twitter, a big part of the problem, I think, is the proliferation of shitty tools. Devs have been trained to neglect security because they’ve only ever used awkward, sloppy frameworks that give you a million ways to shoot yourself in the foot. They don’t realise there’s a better, easier way.

If anything I’ve written here has piqued your interest in Phoenix, allow me to shamelessly plug [my LiveView course](https://phoenixliveview.com/) (currently available at 20% off with the code `BLACKFRIDAY`).

_(This blog post is adapted from some threads I wrote on Twitter [here](https://x.com/ThatArrowsmith/status/1848825943105646815) and [here](https://x.com/ThatArrowsmith/status/1849378212297556087).)_

Want more posts like this in your inbox?

No spam. Unsubscribe any time.
