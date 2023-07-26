---
layout: post
type: post
title: "Hello world!"
date: 2023-07-21
category: life
comments: true
author: "ELYANAH ACO"
tags: [github pages, github, jekyll, ruby,]
---

Hello world! 

This will be my first time writing a blog or a website, and doing this with the underlying
goal of getting better at coding and research is a lofty challenge. So there
will definitely be hiccups along the way as I figure this out.

Why exactly am I doing this, and why on Github pages?

- **Public accountability with flexibility**: I tried using Twitter to document
    my study notes, and needing to trim your thoughts down in 280-character chunks wasn't fun.
    This setup will allow me to blabber on as much as I want to (before editing, of course).
    While I do still use Twitter, it'll primarily just be to get news from the
    select tech users I follow.

- **I want to try some web development**. Web developers may scoff at what they
    just read, but migrating to Github Pages enabled me to learn a bit of web
    dev. Even with just a static site generator like Jekyll, I was able to dip
    my toes on the Rails ecosystem, SCSS, and the like.
- **Open-source experience**. I realized that if readers found any mistakes,
    either from grammar or content, they can just go ahead and open up a Pull Request. 
    They don't need to point them out in the comments and wait for me to
    correct them myself. Github integration is pretty good, and I'd love to
    leverage that[^1].

On the other hand, there are still some things that may need some improvement:

- **Plugins require setup**. For Wordpress, I just virtually need to
    drag and drop plugins I want to have. SEO and Analytics are automatically
    available. With Github Pages, I need to setup Disqus for comments, Google 
    Analytics for stats, and other things. *Github pages doesn't support
    plugins out-of-the-box*, so if I want to do that, I need to push the
    locally generated files instead [^2].
- **No databases, etc.**. That's definitely expected, especially on static-site
    generators. If you want the full blogging experience, I'd say stick to
    other services for that. If you want a clean and lightweight experience,
    then Github pages is a good solution for you.

With that in mind, I won't suggest Jekyll + Github Pages for non-developers who
don't want to roll-up their sleeves and tinker around their site. If you want
an easy and no-ops way of blogging, use other providers such as Medium or
Wordpress. But, if you find joy in building things and making things work, then
Github pages is definitely the best solution for you!

### Footnotes
[^1]: Of course, the source code is available [on Github](https://github.com/ljvmiranda921/ljvmiranda921.github.io)
[^2]: I solved this problem by integrating Travis-CI in my workflow. Whenever the checks on the `master` passed, it automatically deploys the **locally-generated files** to `gh-pages`, and is deployed to the website.
