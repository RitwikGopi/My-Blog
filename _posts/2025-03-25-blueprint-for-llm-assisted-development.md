---
layout: post
title: "Blueprint for LLM Assisted Development"
share: true
image:
  thumbnail: "/assets/2025/llm.jpeg"
excerpt: ""
date: 2025-03-25 23:53:00.000000000 +05:30
type: post
status: publish
categories:
  - Technical
tags:
  - LLM
  - AI
  - State of development
  - unstract
  - open source
  - helm
  - windsurf
  - codeium
permalink: "/2025/03/25/1/"
---

This article outlines a blueprint for software development using LLM-assisted IDEs. The points discussed here are based on experiences using [Windsurf](https://codeium.com/windsurf), a next-generation development environment, for an [open-source project](https://github.com/Zipstack/helm-values-manager) that I am working on. While the examples are drawn from this specific experience, the principles are applicable to any LLM-assisted IDE.

## TLDR: You still need to follow good engineering practices

At the end of the day, successful development with LLMs boils down to good engineering practices. Investing time in learning and applying these practices will allow you to maximize the benefits of the system.

Instead of focusing on specific code and logic, start from a higher level by thinking about what needs to be achieved and the best way to organize the code for that purpose.

>**MOST IMPORTANT POINT IS ALWAYS TO REVIEW THE CODE GENERATED and DO MANUAL TESTS WHEN REQUIRED**

### Requirements Management

It all starts with having clear requirements. Ideally, these requirements should be documented in a way that the LLM can easily access and refer to when needed. I've found that using Architecture Decision Records (ADRs) and design documents works best for this purpose. These documents can even be initially generated with the help of LLMs and then iteratively refined as the project progresses.

Beyond functional requirements, it's crucial to consider non-functional requirements related to engineering excellence. These requirements ensure that the code is robust, performant, and scalable.

Some examples of non-functional requirements include:

- Code quality: The code should be easily understandable and maintainable. Follow clean coding practices and always write code with the intention of making it easy for others to pick up and continue working on the codebase.
- Testability: The code should be easily testable. Write code in a way that makes it easy to write unit tests and integration tests. Follow test-driven development when possible.
- Collaboration: Write code with the intention of making it easy for others to collaborate. This includes following best practices for code reviews, ensuring that all code is reviewed before it is merged into the main branch, and making sure that all code is well documented.
- Robustness: The code should be able to handle invalid user input.
- Security: The code should be secure against common attacks.
- Performance: Consider performance requirements based on high-level project goals and avoid common performance pitfalls.
- Scalability: Consider scalability requirements based on high-level project goals.

Tools like `.windsurfrules` can be used to add and enforce such non-functional requirements.

### Design & Specifications

While requirements define the high-level goals, specifications delve into the lower-level implementation details. Planning these specifications and creating low-level designs and sequence diagrams will serve as valuable reference documents for development. You can instruct the LLM to refer to these documents for implementation details.

Use the LLM to research best practices and generate these designs. If you encounter something you don't understand or believe could be done better, discuss it with the LLM.

### Tasks

In traditional development, we create and assign tasks. Similarly, with the help of an LLM, create a task list. Each task should be granular and self-contained. Especially for larger features, start from the ground up, breaking down the feature into smaller, manageable tasks.

### Bootstrapping

If you're using a framework like Django, bootstrapping it before the actual development begins can be extremely helpful. Additionally, setting up the testing environment early on allows the agents to follow test-driven development principles. Use the LLM to create GitHub workflows for test setup from the beginning, as this can significantly streamline the future development process.

### Code Generation

Now you can pick each task and generate code based on the requirements and specifications. Asking the LLM to be conservative in its code generation can be beneficial. Instruct it not to change tests without specific instructions and to make only minor changes to existing code unless explicitly requested.

### Continuous Improvement

Continuously iterate and improve aspects of the code as you develop. With the help of a robust test rig and the LLM, you can easily iterate and make changes to the code, ensuring ongoing improvement.

## Use it as an Assistant

Use the LLM as an assistant for coding, allowing you to dedicate more time to thinking about the problem at hand. Be aware that LLMs may sometimes "hallucinate" when reasoning at a broader level. Therefore, focus on the high-level thinking yourself. You can use LLMs for discussing ideas and getting suggestions, but ultimately, the decisions regarding design, code, and other aspects should be your own.

Always review every piece of code it generates. Try generating small bits of code to make the review process easier. Also, perform a code review yourself. While the generated code might save you a significant amount of time, you'll still need to invest time in understanding it. Don't accept anything blindly.

## Closing Thoughts

- Use the LLM-assisted IDE in a pair programming manner, taking the driver's seat to guide the coding process.
- Spend more time thinking about the problem.
- Be cautious of hallucinations; always review generated code.
- Generate small bits of code for easier understanding.
- Conduct thorough code reviews before accepting any changes.
