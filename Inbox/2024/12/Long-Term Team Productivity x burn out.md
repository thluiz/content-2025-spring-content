---
created: 2024-10-15T07:46:18 (UTC -03:00)
tags: []
source: chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html
author: Alvaro Lorente
---

# 

> ## Excerpt
> Discover the hidden dangers of continuous sprinting in software development. Learn how queuing theory impacts productivity, the adverse effects of long-term high utilization, and strategies to maintain a balanced, efficient, and creative development team.

---
## Long-Term Team Productivity

### The Risks of Continuous Sprinting and How to Prevent Team Burnout

[![](https://substackcdn.com/image/fetch/w_80,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffc66e68c-e98a-4360-8c28-d4e6385d601f_400x400.jpeg)](https://substack.com/profile/144642325-alvaro-lorente)

It has been almost a year since McKinsey released their developer productivity approach. While I fully disagree with their ideas, it highlights the ongoing interest of companies in continuously improving per-developer productivity.

Regardless of whether you work within an agile or non-agile framework, most teams seem unable to complete the assigned work for each cycle. This indicates that we are maximizing utilization.

## The Queuing Theory Behind Work

As work is a continuous stream of requests, it typically takes the form of a waiting queue and is governed by its principles. This is why queuing theory helps us understand how tasks accumulate and how delays can propagate through the system. By analyzing the flow of tasks and identifying bottlenecks, we can better manage workloads and improve efficiency.

Here are two formulas that actually drive the entire system:

### Kingman's Formula

Kingman's Formula, also known as the VUT equation, is a widely used approximation in queuing theory. It helps estimate the average waiting time in a queue. The formula is given by:

E(Wq)≈(ρ1−ρ)(ca2+cs22)τ\\(\\mathbb E(W\_q) \\approx \\left( \\frac{\\rho}{1-\\rho} \\right) \\left( \\frac{c\_a^2+c\_s^2}{2}\\right) \\tau \\)

Kingman's Formula highlights the impact of variability and utilization on waiting times. As utilization approaches 100%, waiting times increase dramatically, which is why it's crucial to avoid overloading systems.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4fb72926-2d5a-4437-bc54-962a08c49f3e_941x287.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4fb72926-2d5a-4437-bc54-962a08c49f3e_941x287.png)

### Little's Law

Little's Law is a fundamental theorem in queuing theory that relates the average number of items in a queue (L), the average arrival rate of items (λ), and the average time an item spends in the system (W). The law is expressed as:

Little's Law is useful for understanding and managing queues in various contexts, including software development. It implies that to reduce the number of tasks in progress (L), you can either decrease the arrival rate of tasks (λ) or reduce the time tasks spend in the system (W). This principle supports the idea of not overloading development teams and allowing adequate time for tasks to be completed efficiently.

[

![Water Tank Littles Law](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa92e7807-a3e3-4ded-8828-e3d3be32ac69_300x145.png "Water Tank Littles Law")

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa92e7807-a3e3-4ded-8828-e3d3be32ac69_300x145.png)

If we combine both formulas, we can have an obvious image of what happens to our system when a 100% utilization is reached and there is even the slightest amount of variability

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F805947a1-258f-4e0d-823c-c53980810896_882x552.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F805947a1-258f-4e0d-823c-c53980810896_882x552.png)

## Effects of +100% Long-Term Utilization

Now that we have clarified, the math being our task management at work. Let’s discuss the effects of long term sustained high utilization. There are many systems that follow the previous principles, allowing us to observe the stress test effects on both human and non-human systems.

For example, when a CPU or any other system is used at 100% for extended periods or overclocked beyond its standard limits, several adverse effects can occur:

1.  **Heat Generation**: High utilization and overclocking increase the CPU's heat output. Excessive heat can lead to thermal throttling, reducing performance, and in extreme cases, hardware damage.
    
2.  **Reduced Lifespan**: Constant high usage or overclocking can shorten the CPU's lifespan. Increased electrical stress and heat degrade the CPU's materials over time.
    
3.  **System Instability**: Overclocking can cause crashes, freezes, and unexpected reboots. Even at 100% utilization, a system can become unresponsive if it lacks resources.
    
4.  **Increased Power Consumption**: High utilization and overclocking lead to higher power consumption, raising energy costs and stressing the power supply unit (PSU) and cooling systems.
    
5.  **Performance Bottlenecks**: Consistent 100% CPU utilization can bottleneck other components like the GPU, RAM, and storage devices, leading to suboptimal performance.
    
6.  **Noise Levels**: Higher utilization and overclocking often require more aggressive cooling, increasing noise levels and potentially creating a less comfortable working environment.
    

Translating this to humans, constant busyness leaves no time for creative thinking or effective problem-solving, leading to burnout, decreased work quality, and overall productivity loss.

## Preventing the Effects of High Utilization In Humans

Similar to a CPU, there are ways to mitigate these effects:

### Be Strategic About 100% Utilization

Strategic planning for 100% utilization during critical periods can be beneficial but should be done cautiously to avoid long-term negative effects.

-   **Critical Deadlines**: Reserve 100% utilization for critical deadlines or important milestones. Ensure the team understands the importance and temporary nature of this increased workload.
    
-   **Clear Communication**: Communicate clearly about the reasons for the increased workload, its expected duration, and available support. Transparency helps manage expectations and reduce stress.
    
-   **Support Systems**: Provide additional support during these periods, such as extra resources, temporary team members, or access to mental health services.
    

By being strategic about 100% utilization and ensuring adequate cooling-down periods, you can maintain a healthy and productive team, ready to tackle challenges without burning out.

### Cooling Down

Allow periods of reduced workload for recovery and creative thinking to maintain long-term productivity and prevent burnout.

-   **Scheduled Breaks**: Implement regular breaks throughout the workday. Encourage short breaks to rest and recharge, improving focus and productivity.
    
-   **Downtime Projects**: Allocate time for side projects, learning opportunities, or innovation days. This gives a break from usual tasks and fosters creativity.
    
-   **Flexible Work Hours**: Offer flexible work hours or remote work options to accommodate different working styles and needs, leading to a more balanced and satisfied team.
    

### Monitoring and Feedback

Just as monitoring CPU temperatures and stability is crucial, regularly checking in on your team's workload and well-being is essential. This can involve:

-   **Regular Check-ins**: Schedule regular meetings to discuss workload, challenges, and signs of burnout. Encourage open communication and provide a safe space for concerns.
    
-   **Performance Metrics**: Use tools to monitor the team’s performance and workload. Metrics like cycle time, throughput, and work-in-progress (WIP) limits help understand the team’s capacity and adjust workloads accordingly.
    
-   **Feedback Loops**: Establish continuous feedback loops for team input on processes, workload, and job satisfaction. This helps make informed decisions to improve productivity and well-being.
    

### Preventing Bottlenecks

To prevent bottlenecks in your development process, consider these strategies:

-   **Task Prioritization**: Prioritize tasks by urgency and importance. Ensure critical tasks are completed first to avoid overwhelming the team with less important work.
    
-   **Workload Distribution**: Distribute tasks evenly among team members to avoid overburdening individuals, which can lead to delays and decreased productivity.
    
-   **Cross-Training**: Encourage cross-training to ensure multiple team members can handle different tasks, reducing dependency on single individuals and allowing for a more flexible team.
    

## Final Thoughts

It's easy to fall into the trap of continuous sprinting and overutilization at work, so recognizing the importance of balance is crucial. Allowing time for cooldown periods, fostering creativity, and ensuring a healthy work environment are essential for long-term success.

Sustainable productivity is about creating an environment where teams can thrive, innovate, and deliver high-quality work consistently. By being strategic and mindful, we can achieve a more productive and satisfied team ready to tackle any challenge.

Thank you for reading Leads Horizons. This post is public so feel free to share it.

[Share](https://leadshorizons.com/p/long-term-team-productivity?utm_source=substack&utm_medium=email&utm_content=share&action=share)

### Subscribe to Leads Horizons

By Alvaro Lorente · Launched a year ago

Learnings and thoughts about engineering strategy, scaling teams/companies & other software related subjects

#### Discussion about this post
