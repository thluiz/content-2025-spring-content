[Wallaby v2](https://wallabyjs.com/)

## Wallaby GitHub Copilot integration

  18 Feb 2025   6 min read

Which AI model is best for investigating unit test errors? Claude, o3-mini, or GPT-4o? The answer is simple: the **best AI model is the one with the most context** — provided by the user and the right tools.

Imagine a world where you have a failing test, but are limited to reading source code to find out why it’s failing. How much more productive would you be if you could debug the code, explore all runtime values, see execution paths, and test coverage? The same goes for AI models. The more context they have, the better they can help you debug.

Better context wins every time. Today **Wallaby enters the chat** to automatically give AI what it needs - execution paths, test coverage, and runtime values - to debug smarter. We are excited to announce the integration of **Wallaby with GitHub Copilot for VS Code**, but this is [just the beginning](https://wallabyjs.com/blog/wallaby-ai.html#whats-next).

## How to use Wallaby with GitHub Copilot

Simply install [Wallaby](https://marketplace.visualstudio.com/items?itemName=WallabyJs.wallaby-vscode) and [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) (which has recently added a **free tier** for everyone) extensions in your VS Code, start Wallaby, and use the new `Investigate with AI` icon next to the failing test.

Wallaby will open a new Copilot Chat to ask the AI model to investigate the failing test. The AI model will analyze the provided error details. The model may then request additional context from Wallaby on the fly, such as source code, but more importantly:

-   **Code coverage:** Wallaby provides not only the percentage figure / overall coverage of all tests, but also [the exact execution path that led to the failure](https://wallabyjs.com/docs/features/selected-tests/#test-focusing). With this information, the **AI model knows what lines of code were specifically executed by the failing test** and doesn’t need to guess to provide more accurate results.
-   **Runtime values:** Similar to how you can [hover over any source code expression](https://wallabyjs.com/docs/features/advanced-logging/#value-peek) to explore its value with Wallaby, the **AI model can ask Wallaby for the runtime values of any expression in the code** to support its investigation.

Finally, the results of the investigation will be displayed in the chat.

## How does it work

The Wallaby extension registers `@wallaby` Copilot chat participant. When you click on the `Investigate with AI` icon, Wallaby automatically opens a new Copilot Chat with the `@wallaby` participant, uses the selected AI model when possible, and provides the model with the **necessary context and description of Wallaby’s tools** to investigate the failing test.

The initial chat message includes the error details: the error message, stack trace, test name, and instructions for the AI model on how to process the request, including what additional context can be requested from Wallaby on demand.

![](https://wallabyjs.com/assets/img/blog/ai-diagram.png)

The **AI model** is able to analyze the provided error details and **request additional context, like source code, runtime values, execution paths, and test coverage directly from Wallaby**. By default, Wallaby prompts you to allow the AI model to access the requested context, but you can also configure Wallaby to automatically provide the context to the AI model.

Once the model has all the necessary context, it provides the results of the investigation in the chat.

## Tips and tricks

-   The **opened chat can be used after the initial response** to continue to ask the LLM for more investigation, to use Wallaby tools, explain results, correct the response, etc. For example, if you see that the AI model has not used any runtime values or coverage for the investigation of the error, you can ask it to do so:
    -   `@wallaby use runtime tools to get values`
    -   `@wallaby analyze runtime value of variableX`
    -   `@wallaby use coverage for file src/file.ts`

-   If you are using the **Copilot free tier**, please note that because Wallaby integration sends chat messages, [limits may apply](https://docs.github.com/en/copilot/about-github-copilot/subscription-plans-for-github-copilot). Wallaby only sends chat messages when you request, and you may review the number of messages sent by Wallaby on the Wallaby extension page in VS Code.
    
-   If you see that the AI model is not using the provided context effectively, or is outputting errors or strange results, you may **try to re-run the chat with the same or a different AI model**. Currently, Wallaby supports the following AI models: `Claude`, `o3-mini`, `GPT-4o` (default fallback for unsupported models).
    

## Security and privacy considerations

Before the first use of the Wallaby AI integration, you will be prompted by VS Code to allow Wallaby to access Copilot models.

During the initial request of the error investigation feature use, Wallaby will provide the AI model with the necessary context to investigate the failing test:

-   error message;
-   expected and actual values for failed diff assertions;
-   stack trace (each stack entry includes relative file path, line and column number, sometimes function or variable name and/or a single line of code of that stack entry);
-   failing test name.

After the initial request, the AI model may request additional context, like source code, runtime values, and test coverage from Wallaby. By default, Wallaby will prompt you to allow the AI model to access the requested context **with the full description of what is being accessed**, but you can also configure Wallaby to automatically provide the context to the AI model using `wallaby.AI.codeWithCoverageTool` and `wallaby.AI.valueExplorerTool` settings.

If you select a model that you have not used before, and attempt to use it with Wallaby, you may receive an error from Copilot that the model is not available. In this case, you may need to use the model in a separate chat first (with a simple `Hello` message), agree to the displayed Copilot prompt, and then try to use it with Wallaby.

## What’s next

We are considering adding support for similar AI features in [Quokka](https://quokkajs.com/) and [Console Ninja](https://console-ninja.com/).

We also plan to explore the opportunity to expose Wallaby/Quokka/Ninja [tools via MCP](https://modelcontextprotocol.io/introduction) to AI integrations in other IDEs, like **JetBrains AI Assistant, Cursor, Windsurf, Cline**, and other tools.

In addition, we would like to support **more AI-powered workflows** that can benefit from integration with our tools, such as test or implementation code generation powered by Wallaby coverage and runtime values.

## Conclusion

We invite you to explore Wallaby Copilot integration, and share your [feedback with us](https://mail.google.com/mail/?view=cm&fs=1&tf=1&to=hello@wallabyjs.com). Your input is invaluable as we continue to refine and enhance Wallaby to meet your evolving needs.

___