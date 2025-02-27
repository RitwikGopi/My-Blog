---
layout: post
title: "Making sense of python GIL’s impact on threading"
share: true
image:
  thumbnail: "/assets/2025/llm.jpeg"
excerpt: ""
date: 2025-02-27 14:26:00.000000000 +05:30
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
permalink: "/2025/02/27/1/"
---

# LLMs Are Great, but Not Intelligent

## Introduction
Don’t get me wrong—I've been using LLMs and their tools for some time now. I use them daily, and I work at a company that leverages LLMs to solve real pain points for customers([Unstract](https://github.com/Zipstack/unstract) is an open source platform that helps customers extract structured data from unstructured documents with the power of LLMs). So, I’m not saying LLMs are useless. Let’s get that out of the way.

For me, LLMs feel like an awesome evolution of search engine solutions, plus some extras. The purpose of a search engine is to solve one problem: information discovery. When we don’t know something, we Google it, and if the information exists on the internet, we find it. But search engines aren’t always user-friendly for everyone, especially for complex queries. Many technically knowledgeable folks know how to form precise search queries, but for laymen, search engines can still be tricky.

## What Makes LLMs Valuable
LLMs solve this by allowing users to interact with information naturally, like asking a domain expert. They provide clear, understandable answers and can even generate responses not directly found in their training data. This conversational interface makes them more accessible and versatile.

Additionally, LLMs mimic understanding concepts, but while they display traits of intelligence, they are not truly intelligent. When tasked with complex problems that require retaining context and connecting multiple dots, their limitations become evident.

## Challenges with LLMs
A recent experience highlights this. While working on an [open-source](https://github.com/Zipstack/helm-values-manager) project using [Windsurf](https://codeium.com/windsurf), I used LLMs as a pair programmer. They helped generate low-level designs, implementation code, test cases, documentation, and PRs, enhancing my coding experience significantly. However, I encountered roadblocks, such as a stubborn test case error. The LLM kept suggesting solutions, but none worked, and repeated attempts led to even more confusing code changes. Eventually, I discovered the issue myself: an unintentional mock of a file operation. The LLM failed to recognize this due to its lack of true problem-solving ability.

Even at an architectural level, LLMs are helpful only when provided with broad context and correct questions. Without human intervention, their utility is limited.

## Impact on Developers and the Tech Industry
The growing use of LLMs in development raises concerns. Efforts to replace junior engineers with AI could leave the industry without the next generation of senior engineers. Senior engineers can benefit from LLMs by focusing on high-level thinking while delegating routine coding tasks to AI, but human review remains critical.

If we rely solely on AI without training new human talent, we risk running out of experienced engineers to guide AI-driven development. New coding practitioners must find a balance between using LLMs for assistance and independently learning core software engineering principles.

As we integrate LLMs into our workflows, caution is necessary. Blindly copying code—whether from Stack Overflow or an LLM—can lead to future problems. Always review and understand the code you use. The future of software development depends on how well we combine human intelligence with AI assistance.


## Final Thoughts
LLMs have undoubtedly transformed how we approach information and software development. However, their limitations remind us that human intelligence is irreplaceable. As we continue to integrate LLMs into our workflows, it is essential to strike a balance—leveraging AI for efficiency while maintaining human oversight for quality and innovation. The future of tech will be shaped by how well we navigate this relationship between AI tools and human expertise. As developers, our focus should remain on continuous learning, critical thinking, and thoughtful integration of these powerful tools.


**PS**: I have indeed used an LLM to make this article more polished. But again the thoughts for the article are mine. Only how it is articulated has been modified for better readability.


