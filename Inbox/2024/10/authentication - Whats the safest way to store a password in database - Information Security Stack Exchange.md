---
created: 2024-09-12T14:38:27 (UTC -03:00)
tags: []
source: https://security.stackexchange.com/questions/278509/whats-the-safest-way-to-store-a-password-in-database
author: GangSTARclown
        
            32911 silver badge44 bronze badges
---

# authentication - Whats the safest way to store a password in database? - Information Security Stack Exchange

> ## Excerpt
> I read that a password and a salt needs to be combined and then hashed. You save the result and the salt in plaintext. Is it a good practice to use the username as a salt? Why and why not?
I also r...

---
I read that a password and a salt needs to be combined and then hashed. You save the result and the salt in plaintext. Is it a good practice to use the username as a salt? Why and why not?

I also read that good practice is that you need to add a nonce to the password-salt-hash and hash that too. This is done so no replay attacks can be carried out.

The function looks like this:

hash(hash(password+salt)+nounce)

[

![Olivier Jacot-Descombes's user avatar](https://www.gravatar.com/avatar/123d02be3a4fb6074b72b7e28bc05bad?s=64&d=identicon&r=PG)

](https://security.stackexchange.com/users/182939/olivier-jacot-descombes)

[

![GangSTARclown's user avatar](https://www.gravatar.com/avatar/2b24e50de0a12f83c91828416d7581ff?s=64&d=identicon&r=PG&f=y&so-version=2)

](https://security.stackexchange.com/users/321575/gangstarclown)

Hashing the salt doesn't really help, because the point of a salt is to be _unique_, not secret. Using usernames as a salt isn't great either, because the salts should be _globally unique_, and usernames are often re-used between sites.

___

But the fact that you're even thinking about what the salt should be is a bad sign, because proper password hashing algorithms manage all that for you. And any attempt to roll your own crypto or hashing is a **really bad idea**, because there are a lot of subtle things that can go wrong.

The [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) has a lot of good recommendations and guidance on what the current best practices are - so I would start by reading that.

[

![Martijn's user avatar](https://i.sstatic.net/Wmix3.png?s=64)

](https://security.stackexchange.com/users/49124/martijn)

[Martijn](https://security.stackexchange.com/users/49124/martijn)

3611 gold badge2 silver badges9 bronze badges

[

![Gh0stFish's user avatar](https://www.gravatar.com/avatar/a4a29ba46f78d1267cb410e8da5d267d?s=64&d=identicon&r=PG&f=y&so-version=2)

](https://security.stackexchange.com/users/264855/gh0stfish)

[Gh0stFish](https://security.stackexchange.com/users/264855/gh0stfish)

14.6k2 gold badges49 silver badges46 bronze badges

> I also read that good practice is that you need to add a nonce to the password-salt-hash and hash that too. This is done so no replay attacks can be carried out.

This seems to be about a login exchange, not how it's saved in the database. When logging in, the server sends a nonce to the client. The client hashes the password with the nonce and sends it to the server. The server can verify the password. This message cannot be replayed, because then the server sees the nonce for the second time. Authentication to a MySQL server works this way.

This mechanism is not secure. The easiest way to make it secure is to use an authenticated TLS connection. That also solves the replay vulnerability, so this nonce challenge mechnism is no longer needed.

[

![Sjoerd's user avatar](https://www.gravatar.com/avatar/bd5fd2236e22eac8e2cbc8eb2c81ea52?s=64&d=identicon&r=PG)

](https://security.stackexchange.com/users/102236/sjoerd)

[Sjoerd](https://security.stackexchange.com/users/102236/sjoerd)

32.9k13 gold badges86 silver badges116 bronze badges

The safest way is to _not_ store the password at all. Where possible use standards like OIDC or OAuth 2.0 to avoid the need for storing a user's password.

If you really need to store passwords, use a password specific hashing function such as Argon2id. Password specific hashing functions will take care of salting for you, are deliberately slow to limit offline attacks, with the degree of slowness being configurable.

[

![andycaine's user avatar](https://i.sstatic.net/Z8IMRKmS.jpg?s=64)

](https://security.stackexchange.com/users/316766/andycaine)

This is not secure.

First off, as Sjoerd already pointed out, your question title about _storing_ a password hash is detached from the question text which deals with a password-based authentication scheme. Those are two entirely different aspects.

The authentication scheme you've proposed is not secure for many reasons. It actually seems to be a reinvention of the old [Digest Access Authentication](https://en.wikipedia.org/wiki/Digest_access_authentication) scheme and has many of the same problems.

-   The server has to store the password or password-equivalent data as plaintext. Since the server must be able to calculate `hash(hash(password + salt) + nonce)` itself and then compare the result with the client-provided value, they either have to know `password` or `hash(password + salt)`. In the first case, your server is keeping the plaintext passwords of all users, which is a security nightmare. In the second case, the passwords are at least hashed, but knowing the password hash alone is sufficient for an attacker to impersonate the user in your scheme. So with regards to your application, there's no improvement. You're merely doing your users a service by protecting the original passwords in case they've reused them somewhere else (which of course they shouldn't do in the first place).
-   In your scheme, calculating the password hash has to happen _client-side_. Not only does this make JavaScript a strict requirement for your application. It also means you have to find a production-ready JavaScript implementation of a strong password hashing algorithm, you cannot fully control the hash calculations, and you have to be careful not to overload weak client devices like smartphones or tablets – while at the same time making sure the password hashes are strong enough to withstand brute-force attacks.
-   If you use the username as the salt, then an attacker can reuse calculated hashes, potentially allowing them to perform a brute-force attack much more efficiently. For example, if the same username is present in a different application that uses the same hash algorithm, then it's possible to attack both applications at once – the attacker can guess a password, calculate the hash with the username as the salt and then compare the result with the stored hashes in both applications. If the username is very common, the attacker may also have a precomputed table of hashes which lets them search for a match with a simple lookup.
-   Since the entire scheme is home-grown, there's a huge risk of making other mistakes that drastically reduce security.

The proper approach is to transmit the password over a TLS-encrypted connection and use a strong password hash algorithm like [Argon2](https://www.password-hashing.net/#argon2) _on the server_ to calculate the stored hash. While Argon2 is state of the art, it's also fairly new and not yet implemented in the standard libraries of all major programming languages. The are alternatives like [bcrypt](https://en.wikipedia.org/wiki/Bcrypt), though.

Argon2 has different parameters which let you fine-tune the strength of the hashes. You can find [recommended values in RFC 9106](https://www.rfc-editor.org/rfc/rfc9106.html#name-parameter-choice). Note that Argon2 has built-in support for salts, so you just need to make sure you're proving a sufficiently long random value. You should use at least 128 bits obtained from a secure random number generator (most programming languages have a built-in feature for this).

Another interesting feature of Argon2 is that you can mix in a secret value (through the parameter `K`). This way, an attacker can only perform a brute-force attack if they've obtained both the hash _and_ the secret `K`.

[

![Ja1024's user avatar](https://i.sstatic.net/RJ0EI.jpg?s=64)

](https://security.stackexchange.com/users/291964/ja1024)

[Ja1024](https://security.stackexchange.com/users/291964/ja1024)

17.1k2 gold badges45 silver badges53 bronze badges

## You must [log in](https://security.stackexchange.com/users/login?ssrc=question_page&returnurl=https%3a%2f%2fsecurity.stackexchange.com%2fquestions%2f278509) to answer this question.

## 

Not the answer you're looking for? Browse other questions tagged

.
