---
created: 2024-10-02T15:37:36 (UTC -03:00)
tags: [react,javascript,opensource,programming,software,coding,development,engineering,inclusive,community]
source: https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest
author: 
---

# 21 React Projects Too Awesome to Ignore - DEV Community

> ## Excerpt
> AI is going to take over the world soon, we just don't know which part yet.  A few React projects...

---
AI is going to take over the world soon, we just don't know which part yet.

A few React projects already use AI, so I am covering 21 projects that will be very useful for you as a developer.

Some are tools, some are packages but all are open-source so you can at least learn from the codebase.

![Lets go](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9skn6tpk3hisu1fhk99x.gif)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#1-copilotkit-10x-easier-to-build-ai-copilots)1\. [CopilotKit](https://github.com/CopilotKit/CopilotKit) - 10x easier to build AI Copilots.

![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F17l2whb7lpk07vuny7cl.gif)

You will agree that adding AI features in React is tough. That's where Copilot helps you as a framework for building custom AI Copilots in your application.

You can build in-app AI chatbots, and in-app AI Agents with simple components provided by Copilotkit which is at least 10x easier compared to building it from scratch.

You shouldn't reinvent the wheel if there is already a very simple and fast solution!

They also provide built-in (fully-customizable) Copilot-native UX components like `<CopilotKit />`, `<CopilotPopup />`, `<CopilotSidebar />`, `<CopilotTextarea />`.

[![components](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fabbaw6u488uu6xw8y88x.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fabbaw6u488uu6xw8y88x.png)

Get started with the following `npm` command.  

```
npm i @copilotkit/react-core @copilotkit/react-ui
```

This is how you can integrate a Chatbot.

A `CopilotKit` must wrap all components which interact with CopilotKit. You should also start with `CopilotSidebar` (you can swap to a different UI provider later).  

```
<span>"</span><span>use client</span><span>"</span><span>;</span>
<span>import</span> <span>{</span> <span>CopilotKit</span> <span>}</span> <span>from</span> <span>"</span><span>@copilotkit/react-core</span><span>"</span><span>;</span>
<span>import</span> <span>{</span> <span>CopilotSidebar</span> <span>}</span> <span>from</span> <span>"</span><span>@copilotkit/react-ui</span><span>"</span><span>;</span>
<span>import</span> <span>"</span><span>@copilotkit/react-ui/styles.css</span><span>"</span><span>;</span> 

<span>export</span> <span>default</span> <span>function</span> <span>RootLayout</span><span>({</span><span>children</span><span>})</span> <span>{</span>
  <span>return </span><span>(</span>
    <span>&lt;</span><span>CopilotKit</span> <span>url</span><span>=</span><span>"</span><span>/path_to_copilotkit_endpoint/see_below</span><span>"</span><span>&gt;</span>
      <span>&lt;</span><span>CopilotSidebar</span><span>&gt;</span>
        <span>{</span><span>children</span><span>}</span>
      <span>&lt;</span><span>/CopilotSidebar</span><span>&gt;
</span>    <span>&lt;</span><span>/CopilotKit</span><span>&gt;
</span>  <span>);</span>
<span>}</span>
```

You can read the [docs](https://docs.copilotkit.ai/getting-started/quickstart-textarea) and check the [demo video](https://youtu.be/VFXdSQxTTww).

[![copilotkit docs](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8jk7j3uqqv5dflaz1vf5.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8jk7j3uqqv5dflaz1vf5.png)

You can integrate Vercel AI SDK, OpenAI APIs, Langchain, and other LLM providers with ease. You can follow this [guide](https://docs.copilotkit.ai/getting-started/quickstart-chatbot) to integrate a chatbot into your application.

The basic idea is to build AI Chatbots very fast without a lot of struggle, especially with LLM-based apps.

Also check out CopilotKit's CoAgents: A Framework for Human-in-the-Loop!

<iframe width="710" height="399" src="https://www.youtube.com/embed/nBephBv4zr0" allowfullscreen="" loading="lazy"></iframe>

CopilotKit has 8.5k+ stars on GitHub with 300+ releases.

![Star CopilotKit](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fc69t9rs4s706qo3km9pk.gif)

[Star CopilotKit ⭐️](https://github.com/CopilotKit/CopilotKit)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#2-responsively-modified-web-browser-for-5x-faster-responsive-web-development)2\. [responsively](https://github.com/responsively-org/responsively-app/?ref=devtoanmolbaranwal) - modified web browser for 5x faster responsive web development.

[![responsively](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbb5wmmv3ntjluhviajrf.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbb5wmmv3ntjluhviajrf.png)

Responsively is a modified browser built using Electron that helps in responsive web development.

I know Chrome development tools work very well but you will be amazed by how easy it is to use Responsively.

At first glance, anyone would think it of an ordinary app but there are some very unique features:

✅ See all devices at once.

[![all devices at once](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmkqj83m24kg8oukq6o27.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmkqj83m24kg8oukq6o27.png)

✅ Any click, scroll, or navigation you perform on one device will be replicated to all devices in real-time.

![responsively](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fiz2rtwp1a7t31hveaipq.gif)

✅ It allows quick context switching by saving your favorite device combinations as Preview Suits to switch between them when you're testing.

![context switching](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fw2wvpwa21hyrl2vk6eq0.gif)

✅ It comes with a large collection of device profiles out of the box. You can also add your own custom devices.

![profiles](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F47ipnjcg4gboujfu0ely.gif)

You must be thinking about the usual features that would be missing but that's not the case.

⚡ You will get all the basic browser features like cookie management, local storage, session storage, and bookmarks. You can also use the dev tools as in any browser.

⚡ You can also take screenshots of individual devices by clicking on the camera icon in the device toolbar with a quick screenshot of the viewport too.

⚡ The Unified Inspector allows you to inspect elements across all devices at once by pressing `Cmd/Ctrl + I` to activate it and hover over any element to see its details.

![unified inspector](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9lsywvel9etnuffg7z7l.gif)

You can download the [desktop app](https://responsively.app/download/?ref=devtoanmolbaranwal).

[![download options](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzje87s4tnovz2xgqyy9g.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzje87s4tnovz2xgqyy9g.png)

They have also provided [browser extensions](https://github.com/responsively-org/responsively-app?tab=readme-ov-file#browser-extension) for Chrome, Edge, and Firefox to easily send links from your browser to the app and preview instantly.

I would highly recommend to at least check it out because it supports hot reloading as well. It might end up making your workflow much smoother.

Responsively has 22k stars on GitHub and is built using JavaScript.

[Star responsively ⭐️](https://github.com/responsively-org/responsively-app/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#3-drawdb-intuitive-online-database-design-tool-and-sql-generator)3\. [drawdb](https://github.com/drawdb-io/drawdb/?ref=devtoanmolbaranwal) - intuitive online database design tool and SQL generator.

[![drawdb](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ft85tcby1ipwrea2ciami.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ft85tcby1ipwrea2ciami.png)

DrawDB is a robust and user-friendly database entity relationship (DBER) editor right in your browser. Build diagrams with a few clicks, export SQL scripts, customize your editor, and more without creating an account.

There are a lot of features which you can explore yourself.

[![features](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxqwxj23fhn2e0vfq5ome.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxqwxj23fhn2e0vfq5ome.png)

You can try it in the [editor](https://www.drawdb.app/editor/?ref=devtoanmolbaranwal).

[![editor](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9nbhzg4wn93motmjxx4f.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9nbhzg4wn93motmjxx4f.png)

Please note that the diagrams are saved in your browser. Before clearing the browser, just make sure to back up your data.

They have 18.6k stars on GitHub.

[Star drawdb ⭐️](https://github.com/drawdb-io/drawdb/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#4-tooljet-lowcode-platform-for-building-business-applications)4\. [Tooljet](https://github.com/ToolJet/ToolJet?ref=anmolbaranwal) - Low-code platform for building business applications.

[![tooljet](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxhipvjl2wnthjccgrpij.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxhipvjl2wnthjccgrpij.png)

We all build frontend but it is generally way complex and a lot of factors are involved. A lot of people say it's easy but it's not.

ToolJet is a low-code framework to build and deploy internal tools with minimal engineering effort.

Their drag-and-drop frontend builder helps you to create complex, responsive frontends within minutes.

You can integrate:

\-→ Data sources including databases like PostgreSQL, MongoDB and Elasticsearch.  
\-→ API endpoints with OpenAPI spec and OAuth2 support.  
\-→ SaaS tools such as Stripe, Slack, Google Sheets, Airtable, and Notion.  
\-→ Object storage services like S3, GCS, and Minio, to fetch and write data.

Basically, you can use it with everything :)

By the way, ToolJet can also integrate with OpenAI to access two main services. The Completions service enables ToolJet to produce text from a given prompt or context. Meanwhile, the Chat service facilitates user interaction with an AI-driven chatbot based on OpenAI's language models. You can [read more](https://docs.tooljet.com/docs/marketplace/plugins/marketplace-plugin-openai/?ref=devtoanmolbaranwal/) on the docs.

This is how Tooljet works.

[![tooljet](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fr6vv09z7ioma1ce2ttei.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fr6vv09z7ioma1ce2ttei.png)

You can develop multi-step workflows in ToolJet to automate business processes. In addition to building and automating workflows, ToolJet allows for easy integration of these workflows within your applications.

[![workflow](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Feh2vk3kih9fhck6okf67.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Feh2vk3kih9fhck6okf67.png)

You can read this [quickstart guide](https://docs.tooljet.com/docs/getting-started/quickstart-guide/?ref=devtoanmolbaranwal) that shows you how to create an employee directory application in minutes using ToolJet. This app will let you track and update employee information with a beautiful user interface.

See the [list of features](https://github.com/ToolJet/ToolJet?tab=readme-ov-file#all-features) that is available including 45+ built-in responsive components, 50+ data sources, and a whole lot more.

You can read the [docs](https://docs.tooljet.com/docs//?ref=devtoanmolbaranwal) and see the [How to guides](https://docs.tooljet.com/docs/how-to/use-url-params-on-load/?ref=devtoanmolbaranwal).

They have received funding from GitHub so that builds a huge trust.

They have 28.5k stars on GitHub and are built on JavaScript.

[Star ToolJet ⭐️](https://github.com/ToolJet/ToolJet/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#5-appflowy-open-source-alternative-to-notion)5\. [AppFlowy](https://github.com/AppFlowy-IO/AppFlowy/?ref=devtoanmolbaranwal) - open source alternative to Notion.

[![appflowy](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdovisje3bh7ec1h9uqau.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdovisje3bh7ec1h9uqau.png)

AppFlowy is an AI-powered secure workspace similar to the notion where you achieve more without losing control of your data.

[![product](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ful096wqbsxrs8shvwp6c.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ful096wqbsxrs8shvwp6c.png)

You can organize and visualize your data in tables, boards, and calendars. They also provide mobile apps so that's a plus.

You can read the [docs](https://docs.appflowy.io/docs/?ref=devtoanmolbaranwal) and read about [installation methods](https://docs.appflowy.io/docs/appflowy/install-appflowy/installation-methods/?ref=devtoanmolbaranwal).

They support [self-hosting AppFlowy using Supabase](https://docs.appflowy.io/docs/guides/appflowy/?ref=devtoanmolbaranwal). This is ideal for users who use Supabase for their infrastructure.

You should also check [this](https://docs.appflowy.io/docs/appflowy/product/data-storage/?ref=devtoanmolbaranwal) to learn more about data storage, markdown, shortcuts, themes, AI involved, and plugins.

AppFlowy has 51k+ stars on GitHub.

[Star AppFlowy ⭐️](https://github.com/AppFlowy-IO/AppFlowy/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#6-flowise-drag-amp-drop-ui-to-build-your-customized-llm-flow)6\. [Flowise](https://github.com/FlowiseAI/Flowise/?ref=devtoanmolbaranwal) - Drag & drop UI to build your customized LLM flow.

[![flowiseai](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fr5bp43nil764fhe4a05z.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fr5bp43nil764fhe4a05z.png)

Flowise is an open-source UI visual tool to build your customized LLM orchestration flow & AI agents.

We shouldn't compare any projects but I can confidently say this might be the most useful one among the projects listed here!

[![flowise gif](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F141kawm9jjhcgmzam89q.gif)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F141kawm9jjhcgmzam89q.gif)

Get started with the following npm command.  

```
npm install -g flowise
npx flowise start
OR
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

This is how you integrate the API.  

```
<span>import</span> <span>requests</span>

<span>url</span> <span>=</span> <span>"</span><span>/api/v1/prediction/:id</span><span>"</span>

<span>def</span> <span>query</span><span>(</span><span>payload</span><span>):</span>
  <span>response</span> <span>=</span> <span>requests</span><span>.</span><span>post</span><span>(</span>
    <span>url</span><span>,</span>
    <span>json</span> <span>=</span> <span>payload</span>
  <span>)</span>
  <span>return</span> <span>response</span><span>.</span><span>json</span><span>()</span>

<span>output</span> <span>=</span> <span>query</span><span>({</span>
  <span>question</span><span>:</span> <span>"</span><span>hello!</span><span>"</span>
<span>)}</span>
```

[![integrations](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fahk2ovjrpq1qk3r5pfot.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fahk2ovjrpq1qk3r5pfot.png)

You can read the [docs](https://docs.flowiseai.com/?ref=devtoanmolbaranwal).

Cloud host is not available so you would have to self-host using these [instructions](https://github.com/FlowiseAI/Flowise?tab=readme-ov-file#-self-host).

Let's explore some of the use cases:

⚡ Let's say you have a website (could be a store, an e-commerce site, or a blog), and you want to scrap all the relative links of that website and have LLM answer any question on your website. You can follow this [step-by-step tutorial](https://docs.flowiseai.com/use-cases/web-scrape-qna/?ref=devtoanmolbaranwal) on how to achieve the same.

[![scraper](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fe91sz2mga5wvc0x2hp2g.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fe91sz2mga5wvc0x2hp2g.png)

⚡ You can also create a custom tool that will be able to call a webhook endpoint and pass in the necessary parameters into the webhook body. Follow this [guide](https://docs.flowiseai.com/use-cases/webhook-tool/?ref=devtoanmolbaranwal) which will be using Make.com to create the webhook workflow.

[![webhook](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fckyivo9dvue461jc9pv4.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fckyivo9dvue461jc9pv4.png)

There are a lot of other use cases such as building a SQL QnA or interacting with API. Explore and build cool stuff!

FlowiseAI has 27.5k stars on GitHub and has more than 14k forks so it has a good overall ratio.

[Star Flowise ⭐️](https://github.com/FlowiseAI/Flowise/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#7-chatfiles-upload-your-file-and-have-a-conversation-with-it)7\. [ChatFiles](https://github.com/guangzhengli/ChatFiles/?ref=devtoanmolbaranwal) - Upload your file and have a conversation with it.

[![ChatFiles](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Flhimajsma8ijyzeknmlg.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Flhimajsma8ijyzeknmlg.png)

It's more like a document chatbot to interact with multiple files and is powered by GPT / Embedding. You can upload any documents and have a conversation with it, the UI is very good considering they have used another famous open-source project for it.

It uses Langchain and [Chatbot-ui](https://github.com/mckaywrigley/chatbot-ui/?ref=devtoanmolbaranwal) under the hood. Built using Nextjs, TypeScript, Tailwind, and Supabase (Vector DB).

If you're wondering about the approach and the technical architecture, then here it is!

[![architecture](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8zbn7h50k6gwxgz6rkaf.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8zbn7h50k6gwxgz6rkaf.png)

The environment is only for trial and supports a maximum file size of 10 MB which is a drawback, if you want a bigger size then you can [install it locally](https://github.com/guangzhengli/ChatFiles?tab=readme-ov-file#how-to-run-locally).

They have provided [starter questions](https://github.com/guangzhengli/ChatFiles/blob/main/doc/Example.md/?ref=devtoanmolbaranwal) that you can use. You can check the [live demo](https://chatfile.vectorhub.org/?ref=devtoanmolbaranwal).

They have 3.2k stars on GitHub and are on the `v0.3` release.

[Star ChatFiles ⭐️](https://github.com/guangzhengli/ChatFiles/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#8-mindsdb-the-platform-for-customizing-ai-from-enterprise-data)8\. [MindsDB](https://github.com/mindsdb/mindsdb/?ref=devtoanmolbaranwal) - The platform for customizing AI from enterprise data.

[![MindsDB](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fi9q3jdswxdx6wqfk0vqw.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fi9q3jdswxdx6wqfk0vqw.png)

MindsDB is the platform for customizing AI from enterprise data.

With MindsDB, you can deploy, serve, and fine-tune models in real-time, utilizing data from databases, vector stores, or applications, to build AI-powered apps - using universal tools developers already know.

With MindsDB and its nearly [200 integrations](https://docs.mindsdb.com/integrations/data-overview/?ref=devtoanmolbaranwal) to data sources and AI/ML frameworks, any developer can use their enterprise data to customize AI for their purpose, faster and more securely.

[![how MindsDB works](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4q1gfmhq43gopdix03gr.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4q1gfmhq43gopdix03gr.png)

You can read the [docs](https://docs.mindsdb.com/?ref=devtoanmolbaranwal) and [quickstart guide](https://docs.mindsdb.com/quickstart-tutorial/?ref=devtoanmolbaranwal) to get started.

They currently support a total of [3 SDKs](https://docs.mindsdb.com/sdks/overview/?ref=devtoanmolbaranwal) that is using using Mongo-QL, Python, and JavaScript.

There are several applications of MindsDB such as integrating with numerous data sources and AI frameworks.

The other common use cases include fine-tuning models, chatbots, alert systems, content generation, natural language processing, classification, regressions, and forecasting. Read more about the [use cases](https://docs.mindsdb.com/use-cases/?ref=devtoanmolbaranwal) and each of them has an architecture diagram with a little info.

[![use cases](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fwuhxzbioqh9a5s9f0w7s.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fwuhxzbioqh9a5s9f0w7s.png)

For instance, the chatbot architecture diagram with MindsDB. You can read about all the [solutions](https://github.com/mindsdb/mindsdb?tab=readme-ov-file#-get-started) provided along with their SQL Query examples.  

```
// SQL Query Example for Chatbot
CREATE CHATBOT slack_bot USING database='slack',agent='customer_support'; 
```

[![chatbot](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fotoqsro02ghqb709yglk.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fotoqsro02ghqb709yglk.png)

Just to tell you about the overall possibilities, I came across a great article on [How to Forecast Air Temperatures with AI + IoT Sensor Data](https://mindsdb.com/blog/how-to-forecast-air-temperatures-with-ai-iot-sensor-data/?ref=devtoanmolbaranwal) with MindsDB.

[![mindsdb](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F82wrjyrkch44taeurv1r.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F82wrjyrkch44taeurv1r.png)

They have 26k stars on GitHub and are on the `v24.8.1.0` with more than 200 releases. By the way, this is the first time I've seen 4 parts in any release as I always followed the semantic release.

[Star MindsDB ⭐️](https://github.com/mindsdb/mindsdb/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#9-twitter-agent-scrape-data-from-social-media-and-chat-with-it-using-langchain)9\. [Twitter Agent](https://github.com/ahmedbesbes/media-agent/?ref=devtoanmolbaranwal) - Scrape data from social media and chat with it using Langchain.

[![twitter agent](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fg8umoek3meg2tjxw9jna.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fg8umoek3meg2tjxw9jna.png)

Media Agent scrapes Twitter and Reddit submissions, summarizes them, and chats with them in an interactive terminal. Such a cool concept!

You can read the [instructions](https://github.com/ahmedbesbes/media-agent?tab=readme-ov-file#run-the-app-locally) to install it locally.

**It is built using:**

-   Langchain 🦜 to build and compose LLMs.
-   ChromaDB to store vectors (a.k.a embeddings) and query them to build conversational bots.
-   Tweepy to connect to your Twitter API and extract Tweets and metadata.
-   Praw to connect to Reddit API.
-   Rich to build a cool terminal UX/UI.
-   Poetry to manage dependencies.

**You can do a lot of cool stuff like:**

-   Scrapes tweets/submissions on your behalf either from a list of user accounts or a list of keywords.
-   Embeds the tweets/submissions using OpenAI.
-   Creates a summary of the tweets/submissions and provides potential questions to answer.
-   Opens a chat session on top of the tweets.
-   Saves the conversation with its metadata.

Watch the demo!

It has close to 111 stars on GitHub and it isn't maintained anymore (the concept is exciting). You can use it to build something better.

[Star Twitter Agent ⭐️](https://github.com/ahmedbesbes/media-agent/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#10-replexica-aipowered-i18n-toolkit-for-react)10\. [Replexica](https://github.com/replexica/replexica/?ref=devtoanmolbaranwal) - AI-powered i18n toolkit for React.

[![replexica](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhtgshukxy927iy37ui33.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhtgshukxy927iy37ui33.png)

The struggle with localization is definitely real, so having a little AI help with that is worth looking at.

Replexica is an i18n toolkit for React, to ship multi-language apps fast. It doesn't require extracting text into JSON files and uses AI-powered API for content processing.

Replexica is a platform, not a library. It's like having a team of translators and localization engineers working for you but without the overhead. All you need is an API key and voila!

A couple of exciting features that make it all worth it.

✅ Replexica automatically translates your app into multiple languages.

✅ Replexica ensures that translations are accurate and contextually correct, that they fit in the UI, and aim to translate better than a human would ever do. I don't trust an AI though!

✅ Replexica keeps your app localized as you add new features (more like a continuous localization).

Some of the i18n formats supported are:

1.  JSON-free Replexica compiler format.
2.  .md files for Markdown content.
3.  Legacy JSON and YAML-based formats.

To give a general idea behind Replexica, here's the only change that's needed to the basic Next.js app to make it multi-language.

Get started with the following npm command.  

```
// install
pnpm add replexica @replexica/react @replexica/compiler

// login to Replexica API.
pnpm replexica auth --login
```

This is how you can use this.  

```
<span>// next.config.mjs</span>

<span>// Import Replexica Compiler</span>
<span>import</span> <span>replexica</span> <span>from</span> <span>'</span><span>@replexica/compiler</span><span>'</span><span>;</span>

<span>/** @type {import('next').NextConfig} */</span>
<span>const</span> <span>nextConfig</span> <span>=</span> <span>{};</span>

<span>// Define Replexica configuration</span>
<span>/** @type {import('@replexica/compiler').ReplexicaConfig} */</span>
<span>const</span> <span>replexicaConfig</span> <span>=</span> <span>{</span>
  <span>locale</span><span>:</span> <span>{</span>
    <span>source</span><span>:</span> <span>'</span><span>en</span><span>'</span><span>,</span>
    <span>targets</span><span>:</span> <span>[</span><span>'</span><span>es</span><span>'</span><span>],</span>
  <span>},</span>
<span>};</span>

<span>// Wrap Next.js config with Replexica Compiler</span>
<span>export</span> <span>default</span> <span>replexica</span><span>.</span><span>next</span><span>(</span>
  <span>replexicaConfig</span><span>,</span>
  <span>nextConfig</span><span>,</span>
<span>);</span>
```

You can read the [quickstart guide](https://docs.replexica.com/quickstart/?ref=devtoanmolbaranwal) and also clearly documented stuff on [what is used under the hood](https://github.com/replexica/replexica?tab=readme-ov-file#whats-under-the-hood).

They have 1.1k stars on GitHub and are built on TypeScript. A project that will save you a lot of time!

[Star Replexica ⭐️](https://github.com/replexica/replexica/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#11-dompurify-a-domonly-superfast-ubertolerant-xss-sanitizer-for-html)11\. [DOMPurify](https://github.com/cure53/DOMPurify/?ref=devtoanmolbaranwal) - a DOM-only, super-fast, uber-tolerant XSS sanitizer for HTML.

[![DOMPurify](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fr846r2hmmw9d9wzvbocz.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fr846r2hmmw9d9wzvbocz.png)

DOMPurify is a DOM-only, super-fast, uber-tolerant XSS sanitizer for HTML, MathML, and SVG. We as developers need it for our apps to keep them secure enough.

DOMPurify sanitizes HTML and prevents XSS attacks.  
You can feed DOMPurify with a string full of dirty HTML and it will return a string (unless configured otherwise) with clean HTML.

DOMPurify will strip out everything that contains dangerous HTML and thereby prevent XSS attacks and other nastiness. It's also damn bloody fast.

They use the technologies the browser provides and turn them into an XSS filter. The faster your browser, the faster DOMPurify will be.

DOMPurify is written in JavaScript and works in all modern browsers. It doesn't break on MSIE or other legacy browsers.

Get started with the following npm command.  

```
npm install dompurify
npm install jsdom

// or use the unminified development version
&lt;script type="text/javascript" src="src/purify.js"&gt;&lt;/script&gt;
```

This is how you can use this.  

```
<span>const</span> <span>createDOMPurify</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>dompurify</span><span>'</span><span>);</span>
<span>const</span> <span>{</span> <span>JSDOM</span> <span>}</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>jsdom</span><span>'</span><span>);</span>

<span>const</span> <span>window</span> <span>=</span> <span>new</span> <span>JSDOM</span><span>(</span><span>''</span><span>).</span><span>window</span><span>;</span>
<span>const</span> <span>DOMPurify</span> <span>=</span> <span>createDOMPurify</span><span>(</span><span>window</span><span>);</span>
<span>const</span> <span>clean</span> <span>=</span> <span>DOMPurify</span><span>.</span><span>sanitize</span><span>(</span><span>'</span><span>&lt;b&gt;hello there&lt;/b&gt;</span><span>'</span><span>);</span>
```

If you run into problems, please refer to [docs](https://github.com/cure53/DOMPurify?tab=readme-ov-file#how-do-i-use-it). They have documented running it using a script or on the server side.

You can see some of the [purification samples](https://github.com/cure53/DOMPurify?tab=readme-ov-file#some-purification-samples-please) and see the [live demo](https://cure53.de/purify/?ref=devtoanmolbaranwal).

Another useful alternative I've found is [validator.js](https://github.com/validatorjs/validator.js/?ref=devtoanmolbaranwal).

They have 13k+ Stars on GitHub and are used by 340k+ developers which makes them ultra credible.

[Star DOMPurify ⭐️](https://github.com/cure53/DOMPurify/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#12-quivr-your-genai-second-brain)12\. [Quivr](https://github.com/QuivrHQ/quivr/?ref=devtoanmolbaranwal) - your GenAI Second Brain.

[![quivr](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhl12fl88mdjmfkfath1t.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhl12fl88mdjmfkfath1t.png)

Quivr, your second brain, utilizes the power of GenerativeAI to be your personal assistant! Think of it as Obsidian, but turbocharged with AI capabilities.

[![stats](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F5a27c2ubbmri0b2xlh1l.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F5a27c2ubbmri0b2xlh1l.png)

You can read the [installation guide](https://github.com/QuivrHQ/quivr?tab=readme-ov-file#getting-started-).

You can read the [docs](https://docs.quivr.app/home/intro/?ref=devtoanmolbaranwal) and see the [demo video](https://github.com/QuivrHQ/quivr?tab=readme-ov-file#demo-highlights-).

![quivr demo](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8ntopbv39rkt5y73usrg.gif)

They could provide a better free tier plan but it's more than enough to test things on your end.

It has 34k+ Stars on GitHub with 220+ releases which means they're constantly improving.

[Star Quivr ⭐️](https://github.com/QuivrHQ/quivr/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#13-appsmith-platform-to-build-admin-panels-internal-tools-and-dashboards)13\. [Appsmith](https://github.com/appsmithorg/appsmith/?ref=devtoanmolbaranwal) - Platform to build admin panels, internal tools, and dashboards.

[![appsmith](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Frt7s0r3wz2leec83cl17.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Frt7s0r3wz2leec83cl17.png)

Admin panels and dashboards are some of the common parts of any software idea (in most cases) and I've tried to build it from scratch which is a lot of pain with unnecessary hard work.

For starters, watch this [YouTube video](https://www.youtube.com/watch?v=NnaJdA1A11s) that explains Appsmith in 100 seconds.

<iframe width="710" height="399" src="https://www.youtube.com/embed/NnaJdA1A11s" allowfullscreen="" loading="lazy"></iframe>

They provide Drag and drop widgets to build UI.  
You can use 45+ customizable widgets to create beautiful responsive UI in minutes without writing a single line of HTML/CSS. Find the [complete list of widgets](https://www.appsmith.com/widgets/?ref=devtoanmolbaranwal).

[![button clicks widgets](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fkqpnnslvsvjl4gifseon.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fkqpnnslvsvjl4gifseon.png)

[![validations](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F489fly7tvknz2uv2mgei.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F489fly7tvknz2uv2mgei.png)

Appsmith enables writing JavaScript code almost everywhere on the GUI inside widget properties, events listeners, queries, and other settings. Appsmith supports writing single-line code within `[[ ]]` and interprets anything written between the brackets as a JavaScript expression.  

```
<span>/*Filter the data array received from a query*/</span>

test

<span>&#123;&#123;</span> <span>QueryName</span><span>.</span><span>data</span><span>.</span><span>filter</span><span>((</span><span>row</span><span>)</span> <span>=&gt;</span> <span>row</span><span>.</span><span>id</span> <span>&gt;</span> <span>5</span> <span>)</span> <span>&#124;</span>

<span>or</span> 

<span>&#123;&#123;</span>
  <span>storeValue</span><span>(</span><span>"</span><span>userID</span><span>"</span><span>,</span> <span>42</span><span>);</span>  
  <span>console</span><span>.</span><span>log</span><span>(</span><span>appsmith</span><span>.</span><span>store</span><span>.</span><span>userID</span><span>);</span> 
  <span>showAlert</span><span>(</span><span>"</span><span>userID saved</span><span>"</span><span>);</span>
<span>&#124;&#124;</span>
```

You need to use Immediately Invoked Function Expression (IIFE) to write multiple lines.

For instance, the invalid and valid code.  

```
<span>// invalid code</span>
<span>/*Call a query to fetch the results and filter the data*/</span>
<span>&#123;&#123;</span> 
   <span>const</span> <span>array</span> <span>=</span> <span>QueryName</span><span>.</span><span>data</span><span>;</span>
   <span>const</span> <span>filterArray</span> <span>=</span> <span>array</span><span>.</span><span>filter</span><span>((</span><span>row</span><span>)</span> <span>=&gt;</span> <span>row</span><span>.</span><span>id</span> <span>&gt;</span> <span>5</span><span>);</span>
   <span>return</span> <span>filterArray</span><span>;</span>
<span>&#124;&#124;</span>

<span>/* Check the selected option and return the value*/</span>
<span>&#123;&#123;</span> 
  <span>if </span><span>(</span><span>Dropdown</span><span>.</span><span>selectedOptionValue</span> <span>===</span> <span>"</span><span>1</span><span>"</span><span>)</span> <span>{</span>
      <span>return</span> <span>"</span><span>Option 1</span><span>"</span><span>;</span>
  <span>}</span> <span>else</span> <span>{</span>
      <span>return</span> <span>"</span><span>Option 2</span><span>"</span><span>;</span>
  <span>}</span>
<span>&#124;&#124;</span>

<span>// valid code</span>
<span>/* Call a query and then manipulate its result */</span>
<span>&#123;&#123;</span> 
  <span>(</span><span>function</span><span>()</span> <span>{</span>
      <span>const</span> <span>array</span> <span>=</span> <span>QueryName</span><span>.</span><span>data</span><span>;</span>
      <span>const</span> <span>filterArray</span> <span>=</span> <span>array</span><span>.</span><span>filter</span><span>((</span><span>row</span><span>)</span> <span>=&gt;</span> <span>row</span><span>.</span><span>id</span> <span>&gt;</span> <span>5</span><span>);</span>
      <span>return</span> <span>filterArray</span><span>;</span>
   <span>})()</span>
<span>&#124;&#124;</span>

<span>/* Verify the selected option and return the value*/</span>

<span>&#123;&#123;</span> 
  <span>(</span><span>function</span><span>()</span> <span>{</span>
      <span>if </span><span>(</span><span>Dropdown</span><span>.</span><span>selectedOptionValue</span> <span>===</span> <span>"</span><span>1</span><span>"</span><span>)</span> <span>{</span>
        <span>return</span> <span>"</span><span>Option 1</span><span>"</span><span>;</span>
      <span>}</span> <span>else</span> <span>{</span>
        <span>return</span> <span>"</span><span>Option 2</span><span>"</span><span>;</span>
      <span>}</span>
   <span>})()</span>
<span>&#124;&#124;</span>
```

You can create anything from simple CRUD apps to complicated multi-step workflows with a few simple steps:

1.  Integrate with a database or API. Appsmith supports the most popular databases and REST APIs.
    
2.  Use built-in widgets to build your app layout.
    
3.  Express your business logic using queries and JavaScript anywhere in the editor.
    
4.  Appsmith supports version control using Git to build apps in collaboration using branches to track and roll back changes. Deploy the app and share it :)
    

[![appsmith](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyltcrmuzwdoydrwyqjpp.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyltcrmuzwdoydrwyqjpp.png)

You can read the [docs](https://docs.appsmith.com/?ref=devtoanmolbaranwal) and [how to guides](https://docs.appsmith.com/connect-data/how-to-guides/?ref=devtoanmolbaranwal) such as how you can connect it to Local Datasource or how to integrate with third-party tools.

You can self-host or use the cloud. They also provide [20+ templates](https://www.appsmith.com/templates/?ref=devtoanmolbaranwal) so you can quickly get started. Some of the useful ones are:

-   [Maintenance Order Management](https://www.appsmith.com/template/Maintenance-Order-Management)
-   [Crypto Live Tracker](https://www.appsmith.com/template/crypto-live-tracker)
-   [Content Management System](https://www.appsmith.com/template/content-management-system)
-   [WhatsApp Messenger](https://www.appsmith.com/template/whatsapp-messenger)

Appsmith has 32k+ stars on GitHub with 200+ releases.

[Star Appsmith ⭐️](https://github.com/appsmithorg/appsmith/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#14-apitable-apioriented-lowcode-platform-for-building-collaborative-apps)14\. [Apitable](https://github.com/apitable/apitable/?ref=devtoanmolbaranwal) - API-oriented low-code platform for building collaborative apps.

[![apitable](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F58syhvpb2fn6hhlyrtst.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F58syhvpb2fn6hhlyrtst.png)

APITable is an API-oriented low-code platform for building collaborative apps and says that it's better than all other Airtable open-source alternatives.

There are a lot of cool features such as:

-   Realtime collab.

![realtime collab](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F58kpvpab2nj92421yvy3.gif)

-   You can generate an automatic form.

![form](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F0jo084gg0cd9xiud3nz3.gif)

-   Unlimited cross-table links.

![crosstable](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fjnvb9sdp3uqrcn55hwug.gif)

-   API first panel.

![api first panel](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7u48ue4rl0q41rhh6bif.gif)

-   Powerful row/column capabilities.

![row column](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fapxqwp84awdbj7cdw5yu.gif)

You can read the complete [list of features](https://github.com/apitable/apitable?tab=readme-ov-file#-features).

You can try out [apitable](https://aitable.ai/?ref=devtoanmolbaranwal) and see the demo of this project at [live Gitpod demo](https://gitpod.io/#https://github.com/apitable/apitable/?ref=devtoanmolbaranwal) of apitable.

You can also read the [installation guide](https://github.com/apitable/apitable?tab=readme-ov-file#installation) to install APITable in your local or cloud computing environment.

[Star Apitable ⭐️](https://github.com/apitable/apitable/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#15-chat-ui-kit-react-build-your-chat-ui-with-react-in-minutes)15\. [Chat UI Kit React](https://github.com/chatscope/chat-ui-kit-react/?ref=devtoanmolbaranwal) - Build your chat UI with React in minutes.

[![chatscope chat ui kit react](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F0ynb25x1se0riwbvq5uv.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F0ynb25x1se0riwbvq5uv.png)

The Chat UI Kit by Chatscope is an open-source UI toolkit for developing web chat applications.  
Even though the project is not widely used, the features are useful for beginners just checking out the project.

[![features](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fm1y87b1clbi00tojxgzi.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fm1y87b1clbi00tojxgzi.png)

Get started with the following npm command.  

```
npm install @chatscope/chat-ui-kit-react
```

This is how you can create a GUI.  

```
<span>import</span> <span>styles</span> <span>from</span> <span>'</span><span>@chatscope/chat-ui-kit-styles/dist/default/styles.min.css</span><span>'</span><span>;</span>
<span>import</span> <span>{</span> <span>MainContainer</span><span>,</span> <span>ChatContainer</span><span>,</span> <span>MessageList</span><span>,</span> <span>Message</span><span>,</span> <span>MessageInput</span> <span>}</span> <span>from</span> <span>'</span><span>@chatscope/chat-ui-kit-react</span><span>'</span><span>;</span>

<span>&lt;</span><span>div</span> <span>style</span><span>=</span><span>&#123;&#123;</span> <span>position</span><span>:</span><span>"</span><span>relative</span><span>"</span><span>,</span> <span>height</span><span>:</span> <span>"</span><span>500px</span><span>"</span> <span>&#124;&#124;</span><span>&gt;</span>
  <span>&lt;</span><span>MainContainer</span><span>&gt;</span>
    <span>&lt;</span><span>ChatContainer</span><span>&gt;</span>       
      <span>&lt;</span><span>MessageList</span><span>&gt;</span>
        <span>&lt;</span><span>Message</span> <span>model</span><span>=</span><span>&#123;&#123;</span>
                 <span>message</span><span>:</span> <span>"</span><span>Hello my friend</span><span>"</span><span>,</span>
                 <span>sentTime</span><span>:</span> <span>"</span><span>just now</span><span>"</span><span>,</span>
                 <span>sender</span><span>:</span> <span>"</span><span>Joe</span><span>"</span>
                 <span>&#124;&#124;</span> <span>/</span><span>&gt;
</span>        <span>&lt;</span><span>/MessageList</span><span>&gt;
</span>      <span>&lt;</span><span>MessageInput</span> <span>placeholder</span><span>=</span><span>"</span><span>Type message here</span><span>"</span> <span>/&gt;</span>        
    <span>&lt;</span><span>/ChatContainer</span><span>&gt;
</span>  <span>&lt;</span><span>/MainContainer</span><span>&gt;
</span><span>&lt;</span><span>/div</span><span>&gt;
</span>
```

You can read the [docs](https://chatscope.io/docs/?ref=devtoanmolbaranwal).  
More [detailed documentation](https://chatscope.io/storybook/react/?path=/docs/documentation-introduction--docs) is present in the storybook.

It provides some handy components like [`TypingIndicator`](https://chatscope.io/storybook/react/?path=/docs/components-typingindicator--docs), [`Multiline Incoming`](https://chatscope.io/storybook/react/?path=/story/components-message--multiline-incoming), and many more.

I know some of you prefer a blog to understand the whole structure, so you can read [How to Integrate ChatGPT with React](https://rollbar.com/blog/how-to-integrate-chatgpt-with-react/?ref=devtoanmolbaranwal) by Rollbar that uses Chat UI Kit React.

Some of the demos that you can see:

-   [Chatbot UI](https://mars.chatscope.io/?ref=devtoanmolbaranwal)
-   [Chat Friends](https://chatscope.io/demo/chat-friends/?ref=devtoanmolbaranwal) - Check this out!

[![chat friends demo snapshot](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F0hyhqti9yl02rludkocy.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F0hyhqti9yl02rludkocy.png)

They have 1.2k stars on GitHub.

[Star Chat UI Kit React ⭐️](https://github.com/chatscope/chat-ui-kit-react/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#16-npm-copilot-cli-tool-for-nextjs-that-can-analyze-logs-in-realtime)16\. [NPM Copilot](https://github.com/whoiskatrin/npm-copilot/?ref=devtoanmolbaranwal) - CLI tool for Next.js that can analyze logs in real-time.

[![npm copilot](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7omx4d2yzub3gx1xmkvh.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7omx4d2yzub3gx1xmkvh.png)

It is a command-line tool that uses OpenAI's GPT-3 language model to provide suggestions for fixing errors in your code.

The CLI tool detects the project type and package manager being used in the current directory.

It then runs the appropriate development server command (e.g., npm run dev, yarn run dev, pnpm run dev) and listens for logs generated by the running application.

When an error is encountered, the CLI tool provides suggestions for error fixes in real-time.

Get started by installing the npm-copilot package with the following npm command.  

```
npm install -g npm-copilot
```

The CLI tool will begin monitoring the logs generated by the Next.js application and provide suggestions for error fixes in real-time.

You can use this command to use it in the project.  

They have 354 stars on GitHub and support `Nextjs`, `React`, `Angular`, and `Vue.js`.

[Star NPM Copilot ⭐️](https://github.com/whoiskatrin/npm-copilot/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#17-reor-selforganizing-ai-notetaking-app)17\. [reor](https://github.com/reorproject/reor/?ref=devtoanmolbaranwal) - Self-organizing AI note-taking app.

[![reor](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fc0x2q2a67bg7gzdekizw.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fc0x2q2a67bg7gzdekizw.png)

This is one of the most exciting projects (in terms of concept) that I've seen so far, especially because it runs models locally.

Reor is an AI-powered desktop note-taking app, it automatically links related notes, answers questions on your notes, and provides semantic search.

Everything is stored locally and you can edit your notes with an Obsidian-like markdown editor. The project hypothesizes that AI tools for thought should run models locally by default.

Reor stands on the shoulders of the giants Ollama, Transformers.js & LanceDB to enable both LLMs and embedding models to run locally. Connecting to OpenAI or OpenAI-compatible APIs like Oobabooga is also supported.

> 🎯 How can it possibly be `self-organizing`?

a. Every note you write is chunked and embedded into an internal vector database.

b. Related notes are connected automatically via vector similarity.

c. LLM-powered Q&A does RAG on the corpus of notes.

d. Everything can be searched semantically.

You can watch the demo here!

![demo](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fk1whpg9m7ubt5xluyf7f.gif)

One way to think about Reor is as a RAG app with two generators: the LLM and the human. In Q&A mode, the LLM is fed retrieved-context from the corpus to help answer a query.

Similarly, in editor mode, the human can toggle the sidebar to reveal related notes "retrieved" from the corpus. This is quite a powerful way of "augmenting" your thoughts by cross-referencing ideas in a current note against related ideas from your corpus.

You can read the [docs](https://www.reorproject.org/docs/?ref=devtoanmolbaranwal) and [download](https://www.reorproject.org/?ref=devtoanmolbaranwal) from the website. Mac, Linux & Windows are all supported.

They have also provided starter guides so they can help you get started.

[![get started guides](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbx3w7nalcwc9egumu0hm.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbx3w7nalcwc9egumu0hm.png)

They have 6.7k stars on GitHub and are built using TypeScript.

[Star reor ⭐️](https://github.com/reorproject/reor/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#18-rowy-lowcode-backend-platform)18\. [Rowy](https://github.com/rowyio/rowy/?ref=devtoanmolbaranwal) - Low-code backend platform.

[![rowy](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzdjmz8snibeve0cvx22j.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzdjmz8snibeve0cvx22j.png)

Manage your database on a spreadsheet UI and build powerful backend cloud functions, scalably without leaving your browser. Start like no-code, and extend with code using rowy.

There are many use cases but I'm not covering otherwise this will be too long. All the how-to guides and more are present in the docs.

It works with all the major frameworks and any no-code tools like Bubble and Flutterflow.

[![frameoworks](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fivwv3samfvah0ijd9t7z.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fivwv3samfvah0ijd9t7z.png)

You can read the [docs](https://docs.rowy.io/?ref=devtoanmolbaranwal) and check the [live demo](https://demo.rowy.io/?ref=devtoanmolbaranwal).

[![live demo](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9wl2uhjdm9hbc8k3gbhl.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9wl2uhjdm9hbc8k3gbhl.png)

Also recommend watching the product walkthrough!

<iframe width="710" height="399" src="https://www.youtube.com/embed/SKF31EN9CEA" allowfullscreen="" loading="lazy"></iframe>

They have 5.9k stars on GitHub.

[Star Rowy ⭐️](https://github.com/rowyio/rowy/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#19-react-cosmos-sandbox-for-developing-and-testing-ui-components-in-isolation)19\. [React Cosmos](https://github.com/react-cosmos/react-cosmos/?ref=devtoanmolbaranwal) - Sandbox for developing and testing UI components in isolation.

[![react cosmos](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Flztrky6whqsglui4rvuy.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Flztrky6whqsglui4rvuy.png)

React Cosmos is a powerful tool for developing and testing UI components in isolation. It allows you to focus on one component at a time, resulting in faster iteration and higher-quality components.

You can build a component library that keeps your project organized and friendly to new contributors. Its sandbox environment and component library capabilities optimize your workflow to help you deliver exceptional UI experiences.

You can install it using this npm command for a nextjs project. You can also use it with Vite, React native, or even a [custom bundler](https://reactcosmos.org/docs/getting-started/custom-bundler/?ref=devtoanmolbaranwal).  

```
npm i -D react-cosmos react-cosmos-next
```

It has all the features to make it a standard.

[![features](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F30k4iui85oxoh0otxof8.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F30k4iui85oxoh0otxof8.png)

You can read the [docs](https://reactcosmos.org/docs/?ref=devtoanmolbaranwal) and check the [live demo](https://reactcosmos.org/demo/?fixtureId=%7B%22path%22%3A%22src%2Fcomponents%2FTodoApp.fixture.tsx%22%7D).

[![live demo](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F2f3z67ujeoh1rg2xk02s.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F2f3z67ujeoh1rg2xk02s.png)

They have 8.2k stars on GitHub.

[Star React Cosmos ⭐️](https://github.com/react-cosmos/react-cosmos/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#20-hyper-a-terminal-built-on-web-technologies)20\. [Hyper](https://github.com/vercel/hyper/?ref=devtoanmolbaranwal) - a terminal built on web technologies.

[![hyper](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdz5mvaensa4xwhzrev1v.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdz5mvaensa4xwhzrev1v.png)

Let's be honest, most of the terminals are very bad in terms of interface and they just do the work.

Warp is a good one but it's not available for Windows and I've been using Hyper for almost 3 years.

Hyper was created as a beautiful and extensible experience for command-line interface users, built on open web standards.

I've configured it based on my preference. If you want, you can check the config in this [secret gist](https://gist.github.com/Anmol-Baranwal/4059b062a71652faafef07e2a6ba3df3). I only use a single plugin which is called hyperpower.

It lets me use Linux commands on Windows which I did years ago when I was starting.

[![my hyper terminal](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbcxkqjvv4ykgsimwonjp.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbcxkqjvv4ykgsimwonjp.png)

  

my hyper terminal

Many community members use Hyper so there is an extensive collection of 239+ [themes, plugins, and resources](https://github.com/bnb/awesome-hyper/?ref=devtoanmolbaranwal). You can also find it on the [website](https://hyper.is/plugins/?ref=devtoanmolbaranwal).

[![website](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8jqd5meg4dmv1ox2wdva.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8jqd5meg4dmv1ox2wdva.png)

You can find info about all the properties and additional APIs on the website as well.

Hyper has 43k+ stars on GitHub which was kind of shocking to me.

[Star Hyper ⭐️](https://github.com/vercel/hyper/?ref=devtoanmolbaranwal)

___

## [](https://dev.to/copilotkit/21-react-projects-too-awesome-to-ignore-17ec?context=digest#21-remotion-make-videos-using-code)21\. [Remotion](https://github.com/remotion-dev/remotion/?ref=devtoanmolbaranwal) - make videos using code.

[![remotion](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fwmnrxhsc7b9mm5oagflm.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fwmnrxhsc7b9mm5oagflm.png)

Create real MP4 videos using React, and scale your video production using server-side rendering and parametrization.

Get started with the following command.  

It gives you a frame number and a blank canvas where you can render anything you want using React.

This is an example React component that renders the current frame as text.  

```
<span>import</span> <span>{</span> <span>AbsoluteFill</span><span>,</span> <span>useCurrentFrame</span> <span>}</span> <span>from</span> <span>"</span><span>remotion</span><span>"</span><span>;</span>
<span>&nbsp;</span>
<span>export</span> <span>const</span> <span>MyComposition</span> <span>=</span> <span>()</span> <span>=&gt;</span> <span>{</span>
  <span>const</span> <span>frame</span> <span>=</span> <span>useCurrentFrame</span><span>();</span>
<span>&nbsp;</span>
  <span>return </span><span>(</span>
    <span>&lt;</span><span>AbsoluteFill</span>
      <span>style</span><span>=</span><span>&#123;&#123;</span>
        <span>justifyContent</span><span>:</span> <span>"</span><span>center</span><span>"</span><span>,</span>
        <span>alignItems</span><span>:</span> <span>"</span><span>center</span><span>"</span><span>,</span>
        <span>fontSize</span><span>:</span> <span>100</span><span>,</span>
        <span>backgroundColor</span><span>:</span> <span>"</span><span>white</span><span>"</span><span>,</span>
      <span>&#124;&#124;</span>
    <span>&gt;</span>
      <span>The</span> <span>current</span> <span>frame</span> <span>is</span> <span>{</span><span>frame</span><span>}.</span>
    <span>&lt;</span><span>/AbsoluteFill</span><span>&gt;
</span>  <span>);</span>
<span>};</span>
```

You can read the [docs](https://www.remotion.dev/docs/?ref=devtoanmolbaranwal) including the fundamentals.

Check the [list of resources](https://www.remotion.dev/docs/resources/?ref=devtoanmolbaranwal) including templates, SAAS Starter kits, effects, examples, and even full projects.

[![video preview of hello world](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmrj32gjukak4b3dfmov9.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmrj32gjukak4b3dfmov9.png)

Also, check [demo videos](https://www.remotion.dev/showcase/?ref=devtoanmolbaranwal) created during products and campaigns. By the way, the remotion team has been famous for making a GitHub Wrapped for the last 2 years.

You can watch the tutorial demo by Fireship, their videos are just too awesome :)

<iframe width="710" height="399" src="https://www.youtube.com/embed/deg8bOoziaE" allowfullscreen="" loading="lazy"></iframe>

They have 19.5k+ stars on GitHub and are used by 2k+ developers.

[Star Remotion ⭐️](https://github.com/remotion-dev/remotion/?ref=devtoanmolbaranwal)

___

That's all. I hope you found something good enough.

You could use these as an inspiration to build something greater.

Have a great day! Till next time.

By the way, I don't ever use AI to write because I don't want you to read AI content. Would appreciate your follow on Twitter.

You can join my community for developers and tech writers at [dub.sh/opensouls](https://dub.sh/opensouls).

Follow Copilotkit for more content like this.

[

![copilotkit image](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Forganization%2Fprofile_image%2F7820%2Fe8a1bb9a-c520-4645-b24f-06ddf34c44bf.gif)

](https://dev.to/copilotkit)
