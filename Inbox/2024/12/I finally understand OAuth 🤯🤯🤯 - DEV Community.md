---
created: 2024-10-04T15:27:23 (UTC -03:00)
tags: [software,coding,development,engineering,inclusive,community]
source: https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf
author: 
---

# I finally understand OAuth 🤯🤯🤯 - DEV Community

> ## Excerpt
> Let’s be real — OAuth can feel like a mystery wrapped in an enigma. The general idea is simple...

---
Let’s be real — OAuth can feel like a mystery wrapped in an enigma. The general idea is simple enough: “Authenticate users, good things happen, everybody’s happy.” But then you look at the details, and suddenly you're buried in a bunch of seemingly random parameters. Every tutorial online is like, "Just use this," but no one really explains why all these things are there! 🤔

I was in that boat for a long time—until I started building my own authentication library. That's when I had to dig deep into the nitty-gritty, reading through hundreds of pages of OAuth specs (yes, it was as fun as it sounds 😅). But you know what? After all that, I realized OAuth is actually super logical. Every parameter is just to prevent one or more types of attacks. 🛡️

In this article, I’m going to start with the simplest "auth" flow you can imagine — sharing your passwords. From there, we’ll tackle each vulnerability one by one, securing it step by step until we arrive at the full, secure OAuth flow you know and love (or maybe just tolerate).

## [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#stack-auth-opensource-auth0clerk-alternative)Stack Auth: Open-source Auth0/Clerk Alternative

Before we dive in, I want to quickly introduce [Stack Auth](https://github.com/stack-auth/stack), the open-source authentication library we’re building. It’s designed to be super easy to set up and offers a beautiful set of UI components right out of the box! Whether you’re building a SaaS product or your next side project, Stack Auth simplifies authentication without compromising on flexibility.

[![Stack Auth](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fkvs5nw28w741w78hvbcz.png)](https://github.com/stack-auth/stack)

## [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#the-world-without-oauth)The world without OAuth

Big Head wants to save storage space on his Hooli Cloud drive. He finds Pied Piper, an app that promises to compress his files. But for Pied Piper to work its magic, it needs access to his Hooli drive.

The simplest way for Pied Piper to get that access is to ask Big Head for his Hooli username and password. With those, Pied Piper can log into Hooli on his behalf and access his files.

Here's how that would go:

[![Flow 1](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4ftxd6l0imlg4uf2d39z.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4ftxd6l0imlg4uf2d39z.png)

The screen Big Head sees in his browser might look something like this:

[![Flow 2](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxo6z0td531etgj8tekup.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxo6z0td531etgj8tekup.png)

## [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#attack-1-big-heads-credentials-are-exposed)Attack #1: Big Head's credentials are exposed

![We have a problem](https://res.cloudinary.com/practicaldev/image/fetch/s--Gnpabv1y--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExcTRtZjMyOHMxMmRnMGY5emdvbGljemxyZ251OXpyMGM4eGp4NXh2biZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3oD3YQKOzj8cjVoOAg/giphy.webp)

By handing over his username and password, Big Head gives Pied Piper full access to his entire Hooli account. This includes Hooli Mail, Hooli Chat, and even the ability to change his password! Furthermore, if someone ever hacks Pied Piper, they will get to see Big Head's password in plain text.

So, they come up with a new plan: generate an access token. This is a key that lets Pied Piper access Big Head's data on Hooli with limited permissions.

[![Flow 3](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4tjq8rmfw2qumaukvhmx.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4tjq8rmfw2qumaukvhmx.png)

And this is how it would look like to Big Head:

[![Flow 4](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxuckasqgv4103j45qgsf.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxuckasqgv4103j45qgsf.png)

While this approach is secure, it's not particularly user-friendly. Big Head doesn't want to generate an access token manually every single time he compresses a file, or signs in to a service. He wants Pied Piper to do it for him.

### [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#automation)Automation

Instead of Big Head generating access tokens manually and copying them to Pied Piper, Pied Piper can ask Hooli to generate access tokens on his behalf.

[![Flow 5](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxytx0t1sl79fzufgicxn.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxytx0t1sl79fzufgicxn.png)

This would be amazing if there were no bad actors on this world! Big Head just needs to click a button, and everything is done automatically. But, there is an obvious problem here.

## [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#attack-2-anyone-can-just-claim-to-be-anyone-else)Attack #2: Anyone can just claim to be anyone else

![I am your mom](https://res.cloudinary.com/practicaldev/image/fetch/s--R1XBUjHi--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExcDdhYXo3eTBmaGMwdnp5eWVhZmo1bngyZTZyMGx6ZTd3aHFidHF4OSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/26BRuyxFdtlQq5VPa/giphy.webp)

There is absolutely no security in this approach! Pied Piper can just pretend to be acting on behalf of anyone, and Hooli will happily generate an access token.

So, to ensure the request is really from Big Head, Hooli needs to ask him for confirmation.

[![Flow](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzbb3fg9mzwwcsczdnwzb.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzbb3fg9mzwwcsczdnwzb.png)

In the wide world of the web, step 3 is usually done by redirecting Big Head to a page on a `hooli.com` domain. So, his browser would show this:

[![Flow](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4x3p7097q7bhrt4mpfqy.jpg)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4x3p7097q7bhrt4mpfqy.jpg)

Hooli can now verify that the person sitting in front of the browser is Big Head. In step 3, Hooli can freely design the confirmation process. Hooli could ask Big Head to confirm his email/password login, do 2FA, and/or ask for consent.

### [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#putting-a-name-on-it-implicit-flow)Putting a name on it: Implicit flow

Early implementations of OAuth looked exactly like this; we call it the _implicit flow_. Though, we're not even half-way through this post, so maybe you can guess that you shouldn't use this in your production apps.

But, let's start giving names to things, in accordance with what OAuth calls them:

[![Flow](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Flz49tbm167l79gmsq6sb.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Flz49tbm167l79gmsq6sb.png)

The query parameters in the URL are:

-   `client_id=piedpiper`: This tells Hooli which app is asking for access (in this case, Pied Piper).
-   `redirect_uri=https://piedpiper.com/callback`: This is where Hooli should send Big Head back to after he approves the connection.
-   `scope=drive`: The permissions Pied Piper needs.
-   `response_type=token`: Tells Hooli that we are using the implicit flow.

So far so good!

## [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#attack-3-redirect-uri-manipulation)Attack #3: Redirect URI manipulation

Endframe, a malicious competitor to Pied Piper, wants to steal Big Head's Hooli drive data.

1.  Endframe sends Big Head an email with the subject, "Connect your Hooli account with Pied Piper".
    -   They include a link to `https://hooli.com/authorize?...`, with the same `client_id` and `scope` as a real request from Pied Piper.
    -   But, they change the `redirect_uri` parameter to `https://endframe.com`.
2.  Big Head checks the domain and sees it is `hooli.com`, so he clicks on the link.
3.  The Hooli consent screen pops up. Big Head confirms that it is Pied Piper's client ID, so he logs in and confirms access.
4.  However, after Big Head clicks "Allow," Hooli redirects him to `https://endframe.com` instead of `https://piedpiper.com/callback`, sending the access token to Endframe.
5.  Now, Endframe has access to Big Head's Hooli drive!

The problem is that Hooli didn't verify that the `client_id` and `redirect_uri` match.

The solution is to require Pied Piper to register all possible redirect URIs first. Then, Hooli should refuse to redirect to any other domain.

## [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#attack-4-crosssite-request-forgery-csrf)Attack #4: Cross-site request forgery (CSRF)

There are some more advanced attacks however. Endframe can still trick Big Head into logging in with a malicious Hooli account:

1.  Endframe registers an account with Pied Piper.
2.  They log into Pied Piper and generate an access token `access_token_456` for their own Hooli drive.
3.  They send Big Head an email with the title "Check out our Bachmanity photo album" and a link `https://piedpiper.com/callback?access_token=access_token_456`.
4.  Big Head checks the domain, sees it is `piedpiper.com`, and clicks on it.
5.  The Pied Piper website opens the callback and logs in Big Head into Endframe's Hooli drive, so any files he uploads will go to Endframe's Hooli drive.

How can we prevent this? We need to make sure that we only finish the OAuth flow if we initiated it.

To do so, Pied Piper can generate a random string (we call it `state`), store it in cookies, and send it to Hooli. Hooli will then send this `state` back to Pied Piper when it redirects Big Head back. Pied Piper should then check that the received `state` matches the one in the cookies.

[![Flow](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmm3zjrvjp5f9cekxqket.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmm3zjrvjp5f9cekxqket.png)

-   `state=random_string_123`: A randomly generated string to prevent CSRF attacks.

## [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#attack-5-eavesdropping-access-tokens)Attack #5: Eavesdropping access tokens

Endframe isn't giving up. They've come up with another plan:

1.  They teamed up with a genius developer called Jian Yang and created a "Hot dog or not" tool that detects hot dogs. Maliciously, it also sends the entire browser history to Endframe.
    -   (There are a number of other history sniffing or HTTP downgrade attacks that could pull this off, too.)
2.  Big Head thinks it's funny and installs it.
3.  Endframe now sees all the access tokens for all services that Big Head ever logged in with, by searching for URLs that look like `https://piedpiper.com/callback?access_token=access_token_123`.

There is a fix for this. We need to make sure that access tokens are never exchanged in browser URLs.

Hooli generates a short-lived "authorization code" and redirects back to Pied Piper with this code. Pied Piper then makes a POST request to another endpoint, which invalidates the authorization code and exchanges it for the access token.

This way, the access token never ends up in the browser history. We call this the _authorization code_ flow. It is more secure than the implicit flow, but we're still not done.

[![Flow](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbqwt512qubfcgiru5lh9.jpg)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbqwt512qubfcgiru5lh9.jpg)

-   `response_type=code`: Tells Hooli that we are using the authorization code flow, instead of the implicit flow.
-   `code=authorization_code_123`: The authorization code Hooli generated for Pied Piper.
-   `grant_type=authorization_code`: For the authorization code flow, this is always `authorization_code` (we won't cover the other grant types).

## [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#attack-6-eavesdropping-authorization-codes)Attack #6: Eavesdropping authorization codes

If Endframe eavesdrops the authorization code in real-time, they can exchange it for an access token very quickly, before Big Head's browser does.

1.  Big Head still has the "Hot dog or not" tool installed.
2.  As soon as Big Head connects his Hooli drive account, "Hot dog or not" fetches the authorization code from Big Head's browser history.
3.  In real time, faster than Big Head's browser can send the request, Endframe swoops in and sends a request to Hooli with Big Head's authorization code. They get the access token and Big Head's request fails.

Currently, anyone with the authorization code can exchange it for an access token. We need to ensure that only the person who initiated the request can do the exchange.

We do this by securely storing a secret on the client. We hash this secret, and send it to Hooli alongside the very first request. When exchanging the authorization code for the access token, we send the original secret to Hooli. Hooli then compares the hashes, proving that the request is coming from the same client.

This procedure is called _proof key for code exchange_ (PKCE).

[![Flow](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbrydev65thanxbbdt070.jpg)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbrydev65thanxbbdt070.jpg)

-   `code_verifier=random_string_456`: The original random string Pied Piper sent to Hooli.
-   `code_challenge=hashed_string_123`: The hashed code verifier.

Is this secure? Not quite...

## [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#attack-7-redirect-uri-manipulation-on-trusted-uris)Attack #7: Redirect URI manipulation on trusted URIs

Imagine that Pied Piper has two registered redirect URIs:

-   `https://piedpiper.com/callback`
-   `https://piedpiper.com/share-files-callback`

The first one authorizes and compresses Hooli drive files, and the second one shares files publicly with other users. (This could also be the same endpoint with different query parameters.)

Even though we protect against Endframe replacing the redirect URI with something totally malicious (like `https://endframe.com`), if Endframe can intercept the request somehow, they can still modify the URI to point to the other endpoint.

The solution? Make the client send the current URI again when exchanging the authorization code for the access token. Hooli can then verify that the redirect URI matches the one in the original request.

[![Flow](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fan67rw4nbjclkjoneu1m.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fan67rw4nbjclkjoneu1m.png)

## [](https://dev.to/fomalhautb/i-finally-understand-oauth-2ldf#some-final-notes)Some final notes

This is an informal explanation of the OAuth flow, and the actual specification is much longer. For example, client secrets, refresh tokens, client credentials flow, token grants, etc., are not covered here.

Furthermore, there are a lot of nitty-gritty details that are required for a secure implementation. You probably shouldn't implement your own OAuth client.

For diving deeper or if you want to implement OAuth in your apps, here are some good resources.

-   [The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749)
-   [OAuth 2.0 Threat Model and Security Considerations](https://datatracker.ietf.org/doc/html/rfc6819)
-   [OAuth 2.0 for Browser-Based Applications](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-browser-based-apps)

And finally, if you don't want to implement OAuth yourself, that is exactly why we built [Stack Auth](https://github.com/stack-auth/stack), you can get Google/Github/Microsoft/and more OAuth login by just toggling one switch!
