---
created: 2024-09-17T22:39:28 (UTC -03:00)
tags: [bash,git,tutorial,codenewbie,software,coding,development,engineering,inclusive,community]
source: https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest
author: 
---

# Why I Bash Git (And Why You Should Too) - DEV Community

> ## Excerpt
> A lot of people these days use tools like oh-my-zsh that come packed with a ton of helpful features...

---
A lot of people these days use tools like **oh-my-zsh** that come packed with a ton of helpful features out of the box, including Git shortcuts. And don’t get me wrong—they’re great. But I think it’s really important to understand how things work under the hood. You can slap on all the tools you want, but there’s real value in building your own workflow from the ground up.

If you’re curious about my take on **why you should write your own tools**, you can check out my thoughts [here](https://jimmymcbride.dev/blog/writing-your-own-tools). But for now, I want to show you how **Bash functions** and **aliases** can make Git workflows faster, easier, and just plain better. I hope this post gets you excited to dig into your shell’s rc file and start writing your own custom functions and aliases, not just for Git, but for everything you do!

___

### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#1-git-aliases)1\. **Git Aliases**

First up, let’s simplify some of those common Git commands. Here are some aliases I’ve set up to make life a little easier in the terminal. Why type a long command every time when you can shorten it to two letters?  

```
<span>alias </span><span>gs</span><span>=</span><span>"git status"</span>    <span># Show Git status</span>
<span>alias </span><span>ga</span><span>=</span><span>"git add ."</span>     <span># Add all files to the staging area</span>
<span>alias </span><span>gc</span><span>=</span><span>"git commit -m"</span> <span># Commit with a message</span>
<span>alias </span><span>gp</span><span>=</span><span>"git push"</span>      <span># Push the current branch to the remote</span>
<span>alias </span><span>gl</span><span>=</span><span>"git pull"</span>      <span># Pull from the remote branch</span>
<span>alias </span><span>glog</span><span>=</span><span>"git log --oneline --graph --all --decorate"</span> <span># View Git log in one-line format</span>
<span>alias </span><span>gco</span><span>=</span><span>"git checkout"</span> <span># Checkout a branch</span>
<span>alias </span><span>gcb</span><span>=</span><span>"git checkout -b"</span> <span># Create and switch to a new branch</span>
<span>alias </span><span>gd</span><span>=</span><span>"git diff --cached"</span> <span># View the difference of staged changes</span>
<span>alias </span><span>grh</span><span>=</span><span>"git reset --hard HEAD"</span> <span># Hard reset to the latest commit</span>
<span>alias </span><span>gb</span><span>=</span><span>"git branch -vv"</span>  <span># Show branches and last commit in one-line format</span>
<span>alias </span><span>gf</span><span>=</span><span>"git fetch --all"</span> <span># Fetch all remote branches</span>
```

These aliases shave off seconds, but those seconds add up. Plus, they just feel good to use.

___

### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#2-bash-functions-for-more-complex-git-workflows)2\. **Bash Functions for More Complex Git Workflows**

Now, let’s kick it up a notch with some custom **Bash functions** that automate a bit more of your workflow. Functions like these can save you from typing out multiple commands and ensure you don’t miss any steps.

#### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#21-create-a-new-branch-and-push-it)2.1. **Create a New Branch and Push It**

```
gnew<span>()</span> <span>{</span>
  git checkout <span>-b</span> <span>"</span><span>$1</span><span>"</span>
  git push <span>-u</span> origin <span>"</span><span>$1</span><span>"</span>
<span>}</span>
<span># Usage: gnew branch_name</span>
```

#### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#22-quick-commit-and-push)2.2. **Quick Commit and Push**

```
gquick<span>()</span> <span>{</span>
  git commit <span>-am</span> <span>"</span><span>$1</span><span>"</span>
  git push
<span>}</span>
<span># Usage: gquick "commit message"</span>
```

#### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#23-rebase-current-branch-onto-main)2.3. **Rebase Current Branch onto Main**

```
grebase<span>()</span> <span>{</span>
  git fetch
  git rebase origin/main
<span>}</span>
<span># Usage: grebase</span>
```

#### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#24-undo-the-last-commit)2.4. **Undo the Last Commit**

```
gundo<span>()</span> <span>{</span>
  git reset <span>--soft</span> HEAD~1
<span>}</span>
<span># Usage: gundo</span>
```

#### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#25-squash-commits)2.5. **Squash Commits**

```
gsquash<span>()</span> <span>{</span>
  git reset <span>--soft</span> HEAD~<span>"</span><span>$1</span><span>"</span>
  git commit <span>--amend</span>
<span>}</span>
<span># Usage: gsquash 3 (to squash the last 3 commits)</span>
```

#### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#26-sync-fork-with-upstream)2.6. **Sync Fork with Upstream**

```
gupdate-fork<span>()</span> <span>{</span>
  git fetch upstream
  git checkout main
  git merge upstream/main
  git push origin main
<span>}</span>
<span># Usage: gupdate-fork</span>
```

#### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#27-interactive-rebase-on-previous-commits)2.7. **Interactive Rebase on Previous Commits**

```
grebasei<span>()</span> <span>{</span>
  git rebase <span>-i</span> HEAD~<span>"</span><span>$1</span><span>"</span>
<span>}</span>
<span># Usage: grebasei 3 (to interactively rebase the last 3 commits)</span>
```

___

### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#3-general-workflow-enhancers)3\. **General Workflow Enhancers**

These final functions enhance general Git workflows to make things even more efficient.

#### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#31-show-git-tree)3.1. **Show Git Tree**

```
glogtree<span>()</span> <span>{</span>
  git log <span>--graph</span> <span>--oneline</span> <span>--decorate</span> <span>--all</span>
<span>}</span>
<span># Usage: glogtree</span>
```

#### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#32-reset-branch-to-remote)3.2. **Reset Branch to Remote**

```
gresetremote<span>()</span> <span>{</span>
  git fetch origin
  git reset <span>--hard</span> origin/<span>"</span><span>$(</span>git rev-parse <span>--abbrev-ref</span> HEAD<span>)</span><span>"</span>
<span>}</span>
<span># Usage: gresetremote</span>
```

___

### [](https://dev.to/jimmymcbride/why-i-bash-git-and-why-you-should-too-3752?context=digest#4-add-aliases-and-functions-to-your-raw-bashrc-endraw-or-raw-zshrc-endraw-)4\. **Add Aliases and Functions to Your `.bashrc` or `.zshrc`**

If you want these functions and aliases to persist across terminal sessions, you’ll need to add them to your `.bashrc` or `.zshrc`. Here’s how:

1.  Open your shell configuration file:  
    
    ```
    nano ~/.bashrc  <span># OR ~/.zshrc</span>
    ```
    
2.  Paste the aliases and functions into the file.
    
3.  After saving, refresh your shell:  
    
    ```
    <span>source</span> ~/.bashrc  <span># OR ~/.zshrc</span>
    ```
    

___

These are just some of the ways you can make Git work for you, rather than the other way around. By taking a few minutes to tweak your shell setup, you can save hours of typing and clicking over time. **So what about you?**
