---
layout: post
title: "HOST A WEBSITE ON GITHUB"
image:
  thumbnail: "/assets/2021/github-pages.png"
excerpt: ""
date: 2020-12-24 17:90:52.000000000 +05:30
type: post
status: publish
categories:
  - Websites
tags:
  - free hosting
  - hosting
  - hosting website
  - website
  - github
permalink: "/2020/12/24/1/"
---


Hosting a website is something that is somewhat tricky. While some of you may know how to develop a website you might be totally in the dark when it comes to how to  host it. But don't you worry there is an easy and cheap way to do it (of course with some caveats.)

Who can use this method? This will be suitable for someone who wants to host a simple static website or a client side rendered website. Oftentimes beginners who want to showcase their web design skills might not know about this and might end up spending some amount to host their websites. Another group of people are small shop owners who want a very simple page for their shops just to showcase their products/services. Such people can also use github hosting.

So the thing about github hosting is that it is completely free(At least as of when I am writing this article). And one more thing I would like to mention at this point is that you can do the same using gitlab as well.

So let's jump into how to do this. I won't be going in details and turning this article into a full blown tutorial. I believe the documentation and numerous existing articles should be enough for that.

In order to start first you need to create a repo and push your static web code into a branch(I hope the reader has a basic understanding of git. If not it's not rocket science. Just do a quick web search for "[how to use git](https://www.google.com/search?q=how+to+use+git)"). If you are using simple HTML, CSS & JS you can just straight away push it in to your repo. But if you are using something like react, flutter etc create your build and push the artifacts into a branch.

Now all you need to do is go to your repo's settings and enable github pages and then select the branch and path of your index file for your webpage.

And that's all you are done. You have hosted your website on github. Github will give you a subdomain which you can use to access your web page. Now if you are someone looking to customize your website little bit more with a custom domain(For non technical folks if you don't want your web address to looks like <ritwikgopi.github.io> but more like <ritwikg.com>) you need to shell out some bucks and get a domain for yourself. Once you have that you can point your domain to this page using the custom domain option.

Now find out more about github pages & git lab pages from below links

<https://pages.github.com/>

<https://docs.gitlab.com/ee/user/project/pages/>

So you should have realised by now I have hosted this blog you are reading as well on github pages. I have used an awesome tool called jekyll and created this blog. It even helped me import my old wordpress blog content to here. And this theme I used is <https://github.com/mmistakes/so-simple-theme>


You can find more about jekyll at <https://jekyllrb.com/>


I had also done a small project on flutter and hosted the same in the gitlab pages in the past. Not very awesome but you can check it out at <https://connect4.ritwikg.com/#/>

If interested you can find the source code for same at <https://gitlab.com/ritwikgopi/connect-4-flutter>

