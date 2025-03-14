---
created: 2024-09-26T11:09:15 (UTC -03:00)
tags: [webdev,javascript,programming,ai,software,coding,development,engineering,inclusive,community]
source: https://dev.to/nevodavid/8-must-know-open-source-repositories-to-build-cool-ai-apps-4joc?context=digest
author: 
---

# 8 must-know open-source repositories to build cool AI apps 🪄 ✨ - DEV Community

> ## Excerpt
> As someone building AI apps, I see a massive spike in user interest, and this is undoubtedly the best...

---
As someone building AI apps, I see a massive spike in user interest, and this is undoubtedly the best time to master building AI apps.

So, I have compiled a list of 8 open-source repositories you can use now to build robust AI systems.

![Family guy GIF](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fvykhveqc647dfe7g1r98.gif)

___

## [](https://dev.to/nevodavid/8-must-know-open-source-repositories-to-build-cool-ai-apps-4joc?context=digest#1-composio-tooling-infrastructure-for-ai-apps)1\. [Composio](https://dub.composio.dev/edjXUlF) 👑 - Tooling Infrastructure for AI apps

I have been building AI apps for years, and let me tell you, creating a reliable and robust AI app is complex. This is especially true for AI-powered workflow automation.

However, [Composio](https://composio.dev/) makes it a breeze to integrate third-party services, such as GitHub, Slack, Gmail, etc., with AI models for end-to-end workflow automation.

They have over 100 toolsets across business verticals, such as CRM, HRM, Dev, Admin, and Productivity, to help you build complex automation tasks.

They are open-source and have native support for Python and Javascript.

Here’s how to quickly start with Composio.  

```
<span>pip</span> <span>install</span> <span>composio</span><span>-</span><span>core</span>
```

Add a GitHub integration.  

Composio handles user authentication and authorization on your behalf.

Here is how you can use the GitHub integration to star a repository.  

```
<span>from</span> <span>openai</span> <span>import</span> <span>OpenAI</span>
<span>from</span> <span>composio_openai</span> <span>import</span> <span>ComposioToolSet</span><span>,</span> <span>App</span>

<span>openai_client</span> <span>=</span> <span>OpenAI</span><span>(</span><span>api_key</span><span>=</span><span>"</span><span>******OPENAIKEY******</span><span>"</span><span>)</span>

<span># Initialise the Composio Tool Set
</span><span>composio_toolset</span> <span>=</span> <span>ComposioToolSet</span><span>(</span><span>api_key</span><span>=</span><span>"</span><span>**</span><span>\\</span><span>*</span><span>\\</span><span>***COMPOSIO_API_KEY**</span><span>\\</span><span>*</span><span>\\</span><span>***</span><span>"</span><span>)</span>

<span>## Step 4
# Get GitHub tools that are pre-configured
</span><span>actions</span> <span>=</span> <span>composio_toolset</span><span>.</span><span>get_actions</span><span>(</span><span>actions</span><span>=</span><span>[</span><span>Action</span><span>.</span><span>GITHUB_ACTIVITY_STAR_REPO_FOR_AUTHENTICATED_USER</span><span>])</span>

<span>## Step 5
</span><span>my_task</span> <span>=</span> <span>"</span><span>Star a repo ComposioHQ/composio on GitHub</span><span>"</span>

<span># Create a chat completion request to decide on the action
</span><span>response</span> <span>=</span> <span>openai_client</span><span>.</span><span>chat</span><span>.</span><span>completions</span><span>.</span><span>create</span><span>(</span>
<span>model</span><span>=</span><span>"</span><span>gpt-4-turbo</span><span>"</span><span>,</span>
<span>tools</span><span>=</span><span>actions</span><span>,</span> <span># Passing actions we fetched earlier.
</span><span>messages</span><span>=</span><span>[</span>
    <span>{</span><span>"</span><span>role</span><span>"</span><span>:</span> <span>"</span><span>system</span><span>"</span><span>,</span> <span>"</span><span>content</span><span>"</span><span>:</span> <span>"</span><span>You are a helpful assistant.</span><span>"</span><span>},</span>
    <span>{</span><span>"</span><span>role</span><span>"</span><span>:</span> <span>"</span><span>user</span><span>"</span><span>,</span> <span>"</span><span>content</span><span>"</span><span>:</span> <span>my_task</span><span>}</span>
  <span>]</span>
<span>)</span>
```

Run this Python script to execute the given instruction using the agent.

For more about Composio, visit their documentation.

![Composio GIF](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyw87b8mg4uch0ip9dtv4.gif)

[Star the Composio repository ⭐](https://dub.composio.dev/edjXUlF)

___

## [](https://dev.to/nevodavid/8-must-know-open-source-repositories-to-build-cool-ai-apps-4joc?context=digest#2-mem0-the-memory-layer-of-ai-apps)2\. Mem0 - The memory layer of AI apps

Memory handling is a real challenge in building AI assistants. It involves storing and retrieving past interactions. An efficient memory layer is essential for providing personalized user experiences.

Mem0 is by far the best solution in this space. Mem0 remembers user preferences, adapts to individual needs, and continuously improves, making it ideal for customer support chatbots, AI assistants, and autonomous systems.

**Core Features**

-   **Multi-Level Memory**: User, Session, and AI Agent memory retention
-   **Adaptive Personalization**: Continuous improvement based on interactions
-   **Developer-Friendly API**: Simple integration into various applications
-   **Cross-Platform Consistency**: Uniform behaviour across devices
-   **Managed Service**: Hassle-free hosted solution

Start using Mem0 by installing via `pip`.  

Initialize Mem0  

```
from mem0 import Memory
m <span>=</span> Memory<span>()</span>
```

Memory operations.  

```
<span># 1. Add: Store a memory from any unstructured text</span>
result <span>=</span> m.add<span>(</span><span>"I am working on improving my tennis skills. Suggest some online courses."</span>, <span>user_id</span><span>=</span><span>"alice"</span>, <span>metadata</span><span>={</span><span>"category"</span>: <span>"hobbies"</span><span>})</span>

<span># Created memory --&gt; 'Improving her tennis skills.' and 'Looking for online suggestions.'</span>

<span># 2. Update: update the memory</span>
result <span>=</span> m.update<span>(</span><span>memory_id</span><span>=</span>&lt;memory_id_1&gt;, <span>data</span><span>=</span><span>"Likes to play tennis on weekends"</span><span>)</span>

<span># Updated memory --&gt; 'Likes to play tennis on weekends.' and 'Looking for online suggestions.'</span>

<span># 3. Search: search related memories</span>
related_memories <span>=</span> m.search<span>(</span><span>query</span><span>=</span><span>"What are Alice's hobbies?"</span>, <span>user_id</span><span>=</span><span>"alice"</span><span>)</span>

<span># Retrieved memory --&gt; 'Likes to play tennis on weekends'</span>

<span># 4. Get all memories</span>
all_memories <span>=</span> m.get_all<span>()</span>
memory_id <span>=</span> all_memories[<span>"memories"</span><span>][</span>0] <span>[</span><span>"id"</span><span>]</span> <span># get a memory_id</span>

<span># All memory items --&gt; 'Likes to play tennis on weekends.' and 'Looking for online suggestions.'</span>

<span># 5. Get memory history for a particular memory_id</span>
<span>history</span> <span>=</span> m.history<span>(</span><span>memory_id</span><span>=</span>&lt;memory_id_1&gt;<span>)</span>

<span># Logs corresponding to memory_id_1 --&gt; {'prev_value': 'Working on improving tennis skills and interested in online courses for tennis.', 'new_value': 'Likes to play tennis on weekends' }</span>
```

For more information on Mem0 functionalities, refer to their official [documentation](https://docs.mem0.ai/open-source/quickstart).

![Mem0 GIF](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fpuaaeeuv6dsg19fszpjz.gif)

[Star the Mem0 repository ⭐](https://github.com/mem0ai/mem0)

___

## [](https://dev.to/nevodavid/8-must-know-open-source-repositories-to-build-cool-ai-apps-4joc?context=digest#3-agentops-ai-agent-observability-platform)3\. AgentOps - AI Agent observability platform

Building AI agents is now a thing, but ensuring they work as intended is another. Most developers do not bother monitoring their AI agents, which is a disaster in waiting.

AgentOps streamlines agent observability, ensuring the agents are on track. It helps in LLM cost tracking, benchmarking, and more, and it integrates with most LLMs and agent frameworks, such as CrewAI, Langchain, and Autogen.

Get started with AgentOps by installing it through `pip`.  

Initialize the AgentOps client and automatically get analytics on every LLM call.  

```
<span>import</span> <span>agentops</span>

<span># Beginning of program's code (i.e. main.py, __init__.py)
</span><span>agentops</span><span>.</span><span>init</span><span>(</span> <span>&lt;</span> <span>INSERT</span> <span>YOUR</span> <span>API</span> <span>KEY</span> <span>HERE</span> <span>&gt;</span><span>)</span>

<span>...</span>

<span># (optional: record specific functions)
</span><span>@agentops.record_action</span><span>(</span><span>'</span><span>sample function being record</span><span>'</span><span>)</span>
<span>def</span> <span>sample_function</span><span>(...):</span>
    <span>...</span>

<span># End of program
</span><span>agentops</span><span>.</span><span>end_session</span><span>(</span><span>'</span><span>Success</span><span>'</span><span>)</span>
<span># Woohoo You're done 🎉
</span>
```

Refer to their [documentation](https://docs.agentops.ai/v1/introduction) for more.

![AgentOps GIF](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F92kfypu7el99r3unwuhn.gif)

[Star the AgentOps repository ⭐](https://github.com/AgentOps-AI/agentops)

___

## [](https://dev.to/nevodavid/8-must-know-open-source-repositories-to-build-cool-ai-apps-4joc?context=digest#4-e2b-code-interpreting-for-ai-agents)4\. E2B - Code interpreting for AI agents

For many applications, such as building an AI engineer, analyst, or tutor, you will need a way to connect AI models with a code execution environment, and E2B is the leading solution in this space.

E2B Sandbox is a secure cloud environment for AI agents and apps.

It allows AI to run safely for long periods, using the same tools as humans, such as GitHub repositories and cloud browsers.

They offer native Code Interpreter SDKs for Python and Javascript/Typescript.

The Code Interpreter SDK allows you to run AI-generated code in a secure small VM - **E2B sandbox** - for AI code execution. Inside the sandbox is a Jupyter server you can control from their SDK.

Get started with E2B with the following command.  

```
npm i @e2b/code-interpreter
```

Execute a program.  

```
<span>import</span> <span>{</span> <span>CodeInterpreter</span> <span>}</span> <span>from</span> <span>'</span><span>@e2b/code-interpreter</span><span>'</span>

<span>const</span> <span>sandbox</span> <span>=</span> <span>await</span> <span>CodeInterpreter</span><span>.</span><span>create</span><span>()</span>
<span>await</span> <span>sandbox</span><span>.</span><span>notebook</span><span>.</span><span>execCell</span><span>(</span><span>'</span><span>x = 1</span><span>'</span><span>)</span>

<span>const</span> <span>execution</span> <span>=</span> <span>await</span> <span>sandbox</span><span>.</span><span>notebook</span><span>.</span><span>execCell</span><span>(</span><span>'</span><span>x+=1; x</span><span>'</span><span>)</span>
<span>console</span><span>.</span><span>log</span><span>(</span><span>execution</span><span>.</span><span>text</span><span>)</span>  <span>// outputs 2</span>

<span>await</span> <span>sandbox</span><span>.</span><span>close</span><span>()</span>
```

For more on how to work with E2B, visit their official [documentation](https://e2b.dev/docs/hello-world/js).

![E2B Gif](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fc3u9nn6fnghj38i3hi6d.gif)

[Star the AgentOps repository ⭐](https://github.com/e2b-dev/E2B)

___

## [](https://dev.to/nevodavid/8-must-know-open-source-repositories-to-build-cool-ai-apps-4joc?context=digest#5-autogen-a-programming-framework-for-ai-agents)5\. Autogen - A programming framework for AI agents

Building agentic workflows can quickly become overwhelming. Autogen, an open-source framework from Microsoft, simplifies building multi-agent workflows.

They allow you to create AI agents with distinct roles and responsibilities to accomplish complex workflows similar to those of a real-world team.

It supports multiple LLMs and conversation patterns for building effective AI agent swarms.

Quickly get started with Autogen by installing via `pip`.  

A small example of Autogen.  

```
import os
from autogen import AssistantAgent, UserProxyAgent

llm_config <span>=</span> <span>{</span><span>"model"</span>: <span>"gpt-4"</span>, <span>"api_key"</span>: os.environ[<span>"OPENAI_API_KEY"</span><span>]}</span>
assistant <span>=</span> AssistantAgent<span>(</span><span>"assistant"</span>, <span>llm_config</span><span>=</span>llm_config<span>)</span>
user_proxy <span>=</span> UserProxyAgent<span>(</span><span>"user_proxy"</span>, <span>code_execution_config</span><span>=</span>False<span>)</span>

<span># Start the chat</span>
user_proxy.initiate_chat<span>(</span>
    assistant,
    <span>message</span><span>=</span><span>"Tell me a joke about NVDA and TESLA stock prices."</span>,
<span>)</span>
```

For more, refer to their [documentation](https://microsoft.github.io/autogen/docs/Getting-Started).

![Autogen GIF](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F5xrioid0mw8o9imawlrd.gif)

[Star the Autogen repository ⭐](https://github.com/microsoft/autogen)

___

## [](https://dev.to/nevodavid/8-must-know-open-source-repositories-to-build-cool-ai-apps-4joc?context=digest#6-mindsdb-the-platform-for-building-ai-from-enterprise-data)6\. MindsDB - The platform for building AI from enterprise data

If you are building an enterprise application, MindsDB should be the go-to choice. It lets you deploy, serve, and fine-tune models in real-time, utilizing data from databases, vector stores, or applications to build AI-powered apps—using universal tools developers already know.

They act as a bridge between data stored in various data stores with popular AI/ML frameworks such as LangChain, LlamaIndex, and AutoML.

Installing MindsDB locally via [Docker](https://docs.mindsdb.com/setup/self-hosted/docker?utm_medium=community&utm_source=github&utm_campaign=mindsdb%20repo) or [Docker Desktop](https://docs.mindsdb.com/setup/self-hosted/docker-desktop?utm_medium=community&utm_source=github&utm_campaign=mindsdb%20repo), following the instructions in the linked doc pages.

Here is an example of a text-to-SQL agent, which lets you query your data sources with natural language.  

```
<span>--</span> Step 1. Connect a data <span>source </span>to MindsDB
CREATE DATABASE data_source
WITH ENGINE <span>=</span> <span>"postgres"</span>,
PARAMETERS <span>=</span> <span>{</span>
    <span>"user"</span>: <span>"demo_user"</span>,
    <span>"password"</span>: <span>"demo_password"</span>,
    <span>"host"</span>: <span>"samples.mindsdb.com"</span>,
    <span>"port"</span>: <span>"5432"</span>,
    <span>"database"</span>: <span>"demo"</span>,
    <span>"schema"</span>: <span>"demo_data"</span>
<span>}</span><span>;</span>

SELECT <span>*</span>
FROM data_source.car_sales<span>;</span>

<span>--</span> Step 2. Create a skill
CREATE SKILL my_skill
USING
    <span>type</span> <span>=</span> <span>'text2sql'</span>,
    database <span>=</span> <span>'data_source'</span>,
    tables <span>=</span> <span>[</span><span>'car_sales'</span><span>]</span>,
    description <span>=</span> <span>'car sales data of different car types'</span><span>;</span>

SHOW SKILLS<span>;</span>

<span>--</span> Step 3. Deploy a conversational model
CREATE ML_ENGINE langchain_engine
FROM langchain
USING
      openai_api_key <span>=</span> <span>'your openai-api-key'</span><span>;</span>

CREATE MODEL my_conv_model
PREDICT answer
USING
    engine <span>=</span> <span>'langchain_engine'</span>,
    model_name <span>=</span> <span>'gpt-4'</span>,
    mode <span>=</span> <span>'conversational'</span>,
    user_column <span>=</span> <span>'question'</span> ,
    assistant_column <span>=</span> <span>'answer'</span>,
    max_tokens <span>=</span> 100,
    temperature <span>=</span> 0,
    verbose <span>=</span> True,
    prompt_template <span>=</span> <span>'Answer the user input in a helpful way'</span><span>;</span>

DESCRIBE my_conv_model<span>;</span>

<span>--</span> Step 4. Create an agent
CREATE AGENT my_agent
USING
    model <span>=</span> <span>'my_conv_model'</span>,
    skills <span>=</span> <span>[</span><span>'my_skill'</span><span>]</span><span>;</span>

SHOW AGENTS<span>;</span>

<span>--</span> Step 5. Query an agent
SELECT <span>*</span>
FROM my_agent
WHERE question <span>=</span> <span>'what is the average price of cars from 2018?'</span><span>;</span>

SELECT <span>*</span>
FROM my_agent
WHERE question <span>=</span> <span>'what is the max mileage of cars from 2017?'</span><span>;</span>

SELECT <span>*</span>
FROM my_agent
WHERE question <span>=</span> <span>'what percentage of sold cars (from 2016) are automatic/semi-automatic/manual cars?'</span><span>;</span>

SELECT <span>*</span>
FROM my_agent
WHERE question <span>=</span> <span>'is petrol or diesel more common for cars from 2019?'</span><span>;</span>

SELECT <span>*</span>
FROM my_agent
WHERE question <span>=</span> <span>'what is the most commonly sold model?'</span><span>;</span>
```

For more on MondsDB, refer to the [documentation](https://docs.mindsdb.com/what-is-mindsdb).

![Mindsdb GIF](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fi2z39yj8he2ygebhgj2m.gif)

[Star the MindsDB repository ⭐](https://github.com/mindsdb/mindsdb)

___

## [](https://dev.to/nevodavid/8-must-know-open-source-repositories-to-build-cool-ai-apps-4joc?context=digest#7-firecrawl-turn-websites-into-llmready%C2%A0data)7\. FireCrawl - Turn websites into LLM\*-\*ready data

Firecrawl is an open-source scrapping and crawling tool that can convert any webpage into a structured markdown format. This format can subsequently feed LLMs for various applications, such as Q&A, further model fine-tuning, etc.

You can use their API for all the actions.

Here is how to crawl a web page and all the accessible sub-pages.  

```
curl <span>-X</span> POST https://api.firecrawl.dev/v1/crawl <span>\</span>
    <span>-H</span> <span>'Content-Type: application/json'</span> <span>\</span>
    <span>-H</span> <span>'Authorization: Bearer fc-YOUR_API_KEY'</span> <span>\</span>
    <span>-d</span> <span>'{
      "url": "https://docs.firecrawl.dev",
      "limit": 100,
      "scrapeOptions": {
        "formats": ["markdown", "html"]
      }
    }'</span>
```

Scrapping  

```
curl <span>-X</span> POST https://api.firecrawl.dev/v1/scrape <span>\</span>
    <span>-H</span> <span>'Content-Type: application/json'</span> <span>\</span>
    <span>-H</span> <span>'Authorization: Bearer YOUR_API_KEY'</span> <span>\</span>
    <span>-d</span> <span>'{
      "url": "https://docs.firecrawl.dev",
      "formats" : ["markdown", "html"]
    }'</span>
```

Mapping is used to map URLs and get website URLs.  

```
curl <span>-X</span> POST https://api.firecrawl.dev/v1/map <span>\</span>
    <span>-H</span> <span>'Content-Type: application/json'</span> <span>\</span>
    <span>-H</span> <span>'Authorization: Bearer YOUR_API_KEY'</span> <span>\</span>
    <span>-d</span> <span>'{
      "url": "https://firecrawl.dev"
    }'</span>
```

For more information on Firecrawl, refer to the [documentation](https://docs.firecrawl.dev/introduction).

![Firecrwal GIF](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ft02409ffwvvu79aer048.gif)

[Star the FireCrawl repository ⭐](https://github.com/mendableai/firecrawl)

___

## [](https://dev.to/nevodavid/8-must-know-open-source-repositories-to-build-cool-ai-apps-4joc?context=digest#8-lancedb-vector-knowledge-base-for-ai-apps)8\. LanceDB - **Vector knowledge base for AI apps**

If you are building AI apps, you will need a vector database to store and retrieve structured data like text, images, and videos. Unlike traditional databases, vector databases store embeddings of these data.

Embeddings are high-dimensional numerical representations of data. Vector databases use methods like similarity scores to retrieve relevant data.

LanceDb is an open-source vector database written in Typescript. It offers production-scale vector search, multi-modal support, Zero-copy, automatic data versioning, GPU-powered querying, and more.

Get started with LanceDB.  

```
npm <span>install</span> @lancedb/lancedb
```

Create and query a vector database.  

```
<span>import</span> <span>*</span> <span>as</span> <span>lancedb</span> <span>from</span> <span>"</span><span>@lancedb/lancedb</span><span>"</span><span>;</span>

<span>const</span> <span>db</span> <span>=</span> <span>await</span> <span>lancedb</span><span>.</span><span>connect</span><span>(</span><span>"</span><span>data/sample-lancedb</span><span>"</span><span>);</span>
<span>const</span> <span>table</span> <span>=</span> <span>await</span> <span>db</span><span>.</span><span>createTable</span><span>(</span><span>"</span><span>vectors</span><span>"</span><span>,</span> <span>[</span>
    <span>{</span> <span>id</span><span>:</span> <span>1</span><span>,</span> <span>vector</span><span>:</span> <span>[</span><span>0.1</span><span>,</span> <span>0.2</span><span>],</span> <span>item</span><span>:</span> <span>"</span><span>foo</span><span>"</span><span>,</span> <span>price</span><span>:</span> <span>10</span> <span>},</span>
    <span>{</span> <span>id</span><span>:</span> <span>2</span><span>,</span> <span>vector</span><span>:</span> <span>[</span><span>1.1</span><span>,</span> <span>1.2</span><span>],</span> <span>item</span><span>:</span> <span>"</span><span>bar</span><span>"</span><span>,</span> <span>price</span><span>:</span> <span>50</span> <span>},</span>
<span>],</span> <span>{</span><span>mode</span><span>:</span> <span>'</span><span>overwrite</span><span>'</span><span>});</span>

<span>const</span> <span>query</span> <span>=</span> <span>table</span><span>.</span><span>vectorSearch</span><span>([</span><span>0.1</span><span>,</span> <span>0.3</span><span>]).</span><span>limit</span><span>(</span><span>2</span><span>);</span>
<span>const</span> <span>results</span> <span>=</span> <span>await</span> <span>query</span><span>.</span><span>toArray</span><span>();</span>

<span>// You can also search for rows by specific criteria without involving a vector search.</span>
<span>const</span> <span>rowsByCriteria</span> <span>=</span> <span>await</span> <span>table</span><span>.</span><span>query</span><span>().</span><span>where</span><span>(</span><span>"</span><span>price &gt;= 10</span><span>"</span><span>).</span><span>toArray</span><span>();</span>
```

You can find more on LanceDB here on their [documentation](https://lancedb.github.io/lancedb/).

![LanceDB GIF](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fm6aqf7sw9yfgx4e4t2zn.gif)

[Star the LanceDB repository ⭐](https://github.com/lancedb/lancedb)

That's all folks!

___

[![DevFest](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fvtcil4f750nag2pge68r.png)](https://devfest.ai/)

## [](https://dev.to/nevodavid/8-must-know-open-source-repositories-to-build-cool-ai-apps-4joc?context=digest#join-devfest-as-we-celebrate-the-future-of-tech)Join Devfest as we celebrate the future of tech

TL;DR, we created a fantastic event where you can contribute code for any AI repository and get awesome swag.

Join here: [https://devfest.ai](https://devfest.ai/)

The rules are very basic.

-   Go to DevFest AI - create or join an existing squad. Contribute code for any repository containing AI.
-   Collect points, and by the end of the event, 100 people will get awesome swag (the number might go up!)
-   We will also run some fun giveaways for swag during the competition.

And, of course, share your fantastic ticket 🎫

[![Ticket](https://camo.githubusercontent.com/4d57f7e7153c83b5f3e4db4439b2b908084c59253facf2e4c3ab57465179fc42/68747470733a2f2f6465762d746f2d75706c6f6164732e73332e616d617a6f6e6177732e636f6d2f75706c6f6164732f61727469636c65732f643170366a306275396a31346f64393261766e732e706e67)](https://camo.githubusercontent.com/4d57f7e7153c83b5f3e4db4439b2b908084c59253facf2e4c3ab57465179fc42/68747470733a2f2f6465762d746f2d75706c6f6164732e73332e616d617a6f6e6177732e636f6d2f75706c6f6164732f61727469636c65732f643170366a306275396a31346f64393261766e732e706e67)
