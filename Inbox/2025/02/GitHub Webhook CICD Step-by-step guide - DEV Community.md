> (This Blog post is part of a collaborative work between **Me** and **Mustapha El Idrissi**, Consult his devTo page for more information: [https://dev.to/appsbymuss](https://dev.to/appsbymuss))

## [](https://dev.to/techlabma/github-webhook-cicd-step-by-step-guide-1j6g?context=digest#what-is-cicd)What is CI/CD

CI/CD, or **Continuous Integration and Continuous Delivery/Deployment**, is a set of practices and tools that automates the process of software development, testing, and release. It helps developers deliver code changes more frequently, safely, and reliably.

-   **Continuous Integration**: This is a development practice where developers frequently integrate their code changes into a shared repository.
    
-   **Continuous Delivery**: This goes one step further by automating the entire release process. Once code passes all testing stages, it is automatically deployed to the production environment.
    

There are lot of tools that are used to perform **CI** and **CD** such as and not limited to:

-   **Jenkins**
-   **GitHub Actions**
-   **GitLab CI/CD**
-   **Travis CI**

However these tools are usually followed by resource use restrictions or require monetary contributions to be used efficiently and at the same time beginners struggle to use such tools at the start of their Software Development career.

-   There is however an easier-to-bootstrap and harder-to-efficiently-setup way to achieve CI/CD which is **"GitHub [Webhooks](https://www.youtube.com/watch?v=Q_VPL6KrH2o)"**

## [](https://dev.to/techlabma/github-webhook-cicd-step-by-step-guide-1j6g?context=digest#how-to-setup-github-webhook-)How to setup GitHub Webhook ?

#### [](https://dev.to/techlabma/github-webhook-cicd-step-by-step-guide-1j6g?context=digest#step-0-create-a-repo)Step 0: Create a repo

-   Ofcourse when we have a new project we have to create a new repository for it to store our code changes (aka commits), but in this case it will also be useful to achieve our CI/CD goal.

#### [](https://dev.to/techlabma/github-webhook-cicd-step-by-step-guide-1j6g?context=digest#step-1-create-a-route-for-the-postwebhook)Step 1: Create a route for the POST-webhook

-   Assuming that you already have a server with your desired runtime environment/Framework up and running on a specific port.
-   You're gonna have to also make a webhook in order to let Github have a way to reach your server in the case of new change to the **main/production branch on your github repo** like so:

```
<span>require</span><span>(</span><span>'</span><span>dotenv</span><span>'</span><span>).</span><span>config</span><span>();</span>
<span>const</span> <span>express</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>express</span><span>'</span><span>);</span>
<span>const</span> <span>crypto</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>crypto</span><span>'</span><span>);</span>
<span>const</span> <span>bodyParser</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>body-parser</span><span>'</span><span>);</span>
<span>const</span> <span>{</span> <span>exec</span> <span>}</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>child_process</span><span>'</span><span>);</span>
<span>const</span> <span>app</span> <span>=</span> <span>express</span><span>();</span>

<span>const</span> <span>updatedAt</span> <span>=</span> <span>new</span> <span>Date</span><span>();</span>

<span>function</span> <span>verifySignature</span><span>(</span><span>req</span><span>,</span> <span>res</span><span>,</span> <span>buf</span><span>,</span> <span>encoding</span><span>)</span> <span>{</span>
    <span>const</span> <span>signature</span> <span>=</span> <span>req</span><span>.</span><span>headers</span><span>[</span><span>'</span><span>x-hub-signature-256</span><span>'</span><span>];</span> <span>// GitHub sends the signature here</span>

    <span>if </span><span>(</span><span>!</span><span>signature</span><span>)</span> <span>{</span>
        <span>console</span><span>.</span><span>log</span><span>(</span><span>'</span><span>No signature found on request</span><span>'</span><span>);</span>
        <span>return</span> <span>false</span><span>;</span>
    <span>}</span>

    <span>const</span> <span>hmac</span> <span>=</span> <span>crypto</span><span>.</span><span>createHmac</span><span>(</span><span>'</span><span>sha256</span><span>'</span><span>,</span> <span>process</span><span>.</span><span>env</span><span>.</span><span>REPO_WEBHOOK_SECRET</span><span>);</span>
    <span>const</span> <span>digest</span> <span>=</span> <span>'</span><span>sha256=</span><span>'</span> <span>+</span> <span>hmac</span><span>.</span><span>update</span><span>(</span><span>buf</span><span>).</span><span>digest</span><span>(</span><span>'</span><span>hex</span><span>'</span><span>);</span>

    <span>if </span><span>(</span><span>signature</span> <span>!==</span> <span>digest</span><span>)</span> <span>{</span>
        <span>console</span><span>.</span><span>log</span><span>(</span><span>'</span><span>Signature does not match</span><span>'</span><span>);</span>
        <span>return</span> <span>false</span><span>;</span>
    <span>}</span>

    <span>console</span><span>.</span><span>log</span><span>(</span><span>'</span><span>Signature is valid</span><span>'</span><span>);</span>
    <span>return</span> <span>true</span><span>;</span>
<span>}</span>

<span>app</span><span>.</span><span>use</span><span>(</span><span>bodyParser</span><span>.</span><span>json</span><span>());</span>

<span>app</span><span>.</span><span>post</span><span>(</span><span>'</span><span>/cicd/github-cicd</span><span>'</span><span>,</span> <span>(</span><span>req</span><span>,</span> <span>res</span><span>)</span> <span>=&gt;</span> <span>{</span>
    <span>const</span> <span>buf</span> <span>=</span> <span>JSON</span><span>.</span><span>stringify</span><span>(</span><span>req</span><span>.</span><span>body</span><span>);</span> <span>// The raw body of the request</span>
    <span>// const isValid = verifySignature(req, res, buf);</span>

    <span>/* if (!isValid) {
            return res.status(401).send('Invalid signature');
    }*/</span>

    <span>const</span> <span>{</span> <span>ref</span> <span>}</span> <span>=</span> <span>req</span><span>.</span><span>body</span><span>;</span>
    <span>if </span><span>(</span><span>ref</span> <span>===</span> <span>'</span><span>refs/heads/main</span><span>'</span><span>)</span> <span>{</span>
        <span>// PM2 is my instance manager</span>
        <span>exec</span><span>(</span><span>'</span><span>git pull origin main &amp;&amp; pm2 restart cicd_app</span><span>'</span><span>);</span>
    <span>}</span>
    <span>res</span><span>.</span><span>sendStatus</span><span>(</span><span>200</span><span>);</span>
<span>});</span>

<span>app</span><span>.</span><span>get</span><span>(</span><span>'</span><span>/cicd/time</span><span>'</span><span>,</span> <span>(</span><span>req</span><span>,</span> <span>res</span><span>)</span> <span>=&gt;</span> <span>{</span>
    <span>res</span><span>.</span><span>send</span><span>(</span><span>`&lt;h1&gt;</span><span>${</span><span>updatedAt</span><span>}</span><span>&lt;/h1&gt;`</span><span>);</span>
<span>});</span>

<span>app</span><span>.</span><span>listen</span><span>(</span><span>80</span><span>,</span> <span>()</span> <span>=&gt;</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Listening on Port 80...</span><span>"</span><span>);</span>
<span>});</span>
```

([Full source code](https://github.com/0xW3ston/ci_cd_basics))

#### [](https://dev.to/techlabma/github-webhook-cicd-step-by-step-guide-1j6g?context=digest#step-2-configure-the-repos-settings)Step 2: Configure the repo's settings

-   Go to \[YourRepo -> Settings -> Webhooks\]  
    [![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F1ylrjzqbebg5ut9mcjdg.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F1ylrjzqbebg5ut9mcjdg.png)
    
-   Then  
    [![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fq8kx2eo2055ujnbi897q.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fq8kx2eo2055ujnbi897q.png)
    
-   Then we get to the webhook creation panel
    
-   \[**warn**\] Depending on the SSL state of your website (if it's activated or not), choose the "SSL Verification" option accordingly.
    
-   \[**warn**\] Incase you want Github to include a "secret" token to authenticate itself to your server to ensure it's Github and not a threat actor, then put a secret word, otherwise leave empty. 
    

[![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmv6wd4cpwaobh67h7hkm.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmv6wd4cpwaobh67h7hkm.png)

#### [](https://dev.to/techlabma/github-webhook-cicd-step-by-step-guide-1j6g?context=digest#step-3-ready-)Step 3: Ready !

-   After Setting up your webhook on your **webserver** and **github webhook** of your repo, then you're basically good to go.

### [](https://dev.to/techlabma/github-webhook-cicd-step-by-step-guide-1j6g?context=digest#macro)Macro

[![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Faj7a380u8nvbw468fmew.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Faj7a380u8nvbw468fmew.png)

-   **First** a developer will push their commit (code changes) to GitHub.
-   **Secondly** GitHub will send a POST Request to our server, and specifically to our webhook route with the body in JSON with information related to that github push that we've done just a moment ago.
-   **Thirdly** The server will then ofcourse treat said request as a way to know that there are changes on the production branch that must be applied as soon as possible, therefor a **git pull** will be attempted and then tests and new builds and whatnot are going to be executed.
-   **Finally** The server will consequently restart with the updated source code.