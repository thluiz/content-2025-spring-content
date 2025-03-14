---
created: 2024-09-30T10:42:09 (UTC -03:00)
tags: []
source: https://obsidian.rocks/how-to-manage-projects-in-obsidian/
author: Tim Miller
---

# How to Manage Projects in Obsidian - Obsidian Rocks

> ## Excerpt
> How do you manage projects in Obsidian? I like to automate projects as much as possible, and we can do that using the Dataview plugin.

---
> This article is part of a series of tips on handling tasks in Obsidian. To see the full series, see [How to Manage Tasks in Obsidian](https://obsidian.rocks/how-to-manage-tasks-in-obsidian/).

We’ve already discussed _why_ you might want to manage projects in Obsidian, now let’s talk about _how_. How do you set up Obsidian to properly manage projects?

My advice: create a project view. Using a plugin called Dataview, you can create a dynamic dashboard that helps you keep all of your work properly organized.

The project view gives you a wider overview of your current projects, and helps you to keep a broader perspective. The project view is used for review, and for choosing your priorities every day.

This isn’t the only way to manage projects in Obsidian, but for complex multi-task projects, it can be just the thing you need. Let’s dive in.

On This Page \[[hide](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#)\]

-   [1 Limitations](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#Limitations)
-   [2 Prerequisites](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#Prerequisites)
-   [3 Using Tags to Track Project Status](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#Using-Tags-to-Track-Project-Status)
    -   [3.1 Project Templates](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#Project-Templates)
-   [4 Creating Dataview Dashboards to Manage Projects in Obsidian](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#Creating-Dataview-Dashboards-to-Manage-Projects-in-Obsidian)
    -   [4.1 Displaying All Active Projects](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#Displaying-All-Active-Projects)
-   [5 Displaying Ongoing Projects](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#Displaying-Ongoing-Projects)
-   [6 Displaying Upcoming Projects](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#Displaying-Upcoming-Projects)
-   [7 Displaying Archived Projects](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#Displaying-Archived-Projects)
-   [8 Just Use Kanban?](https://obsidian.rocks/how-to-manage-projects-in-obsidian/#Just-Use-Kanban)

## Limitations

Note that this project workflow does not focus on due dates: we created the [today view](https://obsidian.md/Creating%20a%20Today%20View%20in%20Obsidian) specifically to handle due dates, the project view is meant to give you a _high level overview_ of all of your commitments. If you want to handle deadlines and due dates, see [Creating a Today View in Obsidian](https://obsidian.md/Creating%20a%20Today%20View%20in%20Obsidian).

Additionally, this is a tag-based workflow, so you have to remember to keep those tags up-to-date. I recommend you review your project view every week, and that way you ensure that you aren’t missing anything.

(It’s not a bad idea to set up a recurring task for this. If you have a Today view, then it will remind you to review every week)

## Prerequisites

-   **Install and enable** the [Dataview plugin](https://github.com/blacksmithgu/obsidian-dataview) ([here’s how](https://obsidian.rocks/how-to-use-community-plugins-in-obsidian/))

## Using Tags to Track Project Status

If you want an automated project view in Obsidian, then you’ll first need a way to designate the _status_ of a project.

I recommend tags for this. Tags are easy to use, they autocomplete, and Dataview _loves_ them. I have four different tags that I use to track project status. I use these tags to sort projects by priority, similar to how I sort [tasks in my today view](https://obsidian.rocks/creating-a-today-view-in-obsidian/).

Here are the four tags I use:

-   #project/soon 
-   #project/active 
-   #project/routine 
-   #project/archive 

I also use tags to separate my personal from my work projects. To do that, I add one of these two tags to each project:

-   #work
-   #personal

There are a couple of other bits of custom data I add to some projects, including a _standing_, a _priority_, and a _deadline_. I add these using Dataview variables, which look like this:

-   standing:: in progress
-   priority:: 0
-   deadline:: 2024-01-01

### Project Templates

To keep all of this straight, I created a “New Project template”, which I use to create each new project.

If you use the Core Templates plugin, this template should work for you too. Feel free to modify it to fit your needs, the important thing is the tags, and the Dataview variables if you choose to use them:

```
[[Client MOC]]
%% tags: #work #project/soon %%

standing:: coming soon
priority:: 0
deadline:: 2023-01-01

# &#123;&#123;title&#124;&#124;
- [ ] Todos go here
```

> Note: I also hide the tags in preview mode using percentage signs: if you prefer to see the tags, feel free to remove those.

## Creating Dataview Dashboards to Manage Projects in Obsidian

It may seem like a lot of work to get that set up, but it will be worth it.

Once you have a few projects set up in your vault, you can then start to _query_ your projects with Dataview.

I like to create a few separate views. For example, I have a work view and a personal view. Each view displays only what I need when I need it.

This is one of the strengths of using Obsidian for projects: you can customize your views and fine-tune them to show only what you need.

### Displaying All Active Projects

My Active Projects view looks like this:

![](https://i0.wp.com/obsidian.rocks/wp-content/uploads/2022/10/2022-10-13_06-32.png?resize=866%2C603&ssl=1)

That table is created using this Dataview code:

````
```dataview
table standing, priority
from #project/active AND "Projects"
sort priority desc
\```
````

> Note: I keep all my active projects in a “Projects” folder. If you prefer to have your projects spread out (or in a different folder), change the line that says AND “Projects”.

This code creates a table, and includes all my active projects, sorting them by priority.

The main reason I add priority to my projects is so that I can sort them. I like to have my most important project at the top. But you could also sort by `file.name` or `file.mtime` (date last modified).

> Note: if you decide to implement this project view, you might also want to look into [editing Dataview tables inline](https://obsidian.rocks/editing-dataview-tables-with-the-metadata-menu-plugin/). It’s a wonderful addition to this workflow.

## Displaying Ongoing Projects

I like to separate my _active projects_ from my _ongoing projects_. Active projects often have due dates and they are _completed_ and _archived_ once all the tasks are complete.

_Ongoing_ projects are different. I use ongoing projects for chores, routines, areas of interest, anything with repeating tasks and no definite end date.

The goal of an _Active Project_ is to finish it, the goal of an _Ongoing Project_ is to make steady progress over long periods.

To see ongoing projects, I use the tag `#project/ongoing`, and this Dataview code:

````
```dataview
table standing
from #project/ongoing
sort file.name asc
\```
````

I find the order less important here, so I sort alphabetically.

## Displaying Upcoming Projects

After listing my active and ongoing projects, I like to see projects that are coming up. For those projects, I use the tag `#project/soon`, and you can see those projects using this code:

````
```dataview
table standing
from #project/soon
sort file.name asc
\```
````

## Displaying Archived Projects

You may or may not want to list your archived projects. Some people like to see them and to see the list grow over time, others like archived projects to go away and leave them alone.

The choice is yours. If you do want to see your archived projects, you might want to simplify and limit the list. Or create a new view, perhaps called “Archived projects”. For example, here’s a query that spits out a list of your latest archived projects, sorted by modified date, and limited to 15:

````
```dataview
list
from #project/archive
sort file.mtime asc
limit 15
\```
````

## Just Use Kanban?

If you haven’t heard of Kanban before, it’s another method you could use to display this kind of information. There’s an excellent [Kanban community plugin you can use](https://github.com/mgmeyers/obsidian-kanban), if that interests you. It looks like this:

![](https://i0.wp.com/obsidian.rocks/wp-content/uploads/2022/10/2022-10-13_06-38.png?resize=1024%2C569&ssl=1)

This is my “Writing Board”, I like to use Kanban to keep track of articles that I am working on.

Unfortunately, at the time of writing, the Kanban plugin doesn’t support pulling in cards dynamically. It’s still a very useful plugin, but none of the above Dataview queries will work in tandem with Kanban at this point. You have to manage each card manually, which is less useful for me. The [developer is working on fixing this](https://github.com/mgmeyers/obsidian-kanban/issues/550), but until that functionality is available, I prefer a more dynamic solution.

We won't send you spam. Unsubscribe at any time.
