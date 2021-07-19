---
title: Making a Portfolio and Blog Website
date: 2021-07-16 12:00:00 +0000
icon: assets/thumbnails/2021/jekyll.png
alttext: How this website was made
categories: [Blogs, Website]
tags: [website, html, css] # TAG names should always be lowercase
blog: true
---

Hello! If you're reading this, you're looking at my shiny new website. I built this website with two purposes: as a portfolio to showcase my work, and as a blog for me to share things of interest and find my voice.

While it's fresh in my mind, then, let me tell you a bit about how I put this website together!

## Getting familiar with web development

---

I've never been much for web development. To me, it's always seemed like a bunch of black-box technologies connected together that just happen to work, with many points of failure and a lot of things that you just 'need to know'. It's all well and good telling me "you need a domain and hosting, and then you build your website", but as a primarily C++ programmer often focused on optimisations and talking directly to hardware, I get lost with web dev until I understand the whole process of requesting, building, and then receiving a webpage.

Using that as an example, then, I can now tell you roughly how a webpage ends up on your screen: First, your browser sends a request to the given url, which is translated to an IP address by your DNS provider, and sent to a particular port based on convention. The host server is running an application (let's say [Apache](https://httpd.apache.org/) as an example) that receives that request, and processes it to fetch or build the webpage as an HTML structure. It then sends that final structure back to your machine, where the HTML is then displayed by your web browser.

That's surely missing some important steps, but it's enough context for me to comfortably understand what's required to set up a website. For me, the key bit of missing information was that there is a real-time application running on the host server that receives requests, builds pages as HTML structures, and sends them back. That's still a black box for now, but I at least understand the inputs and outputs well enough to get started.

## Picking a framework

---

So, I now understand how the internet works. What now? I don't fancy getting deep into coding a website - I have a simple enough use-case that I've seen done a million times before, so I'd rather not waste my time reinventing the wheel. I've heard the name [Wordpress](https://wordpress.org/) a lot, but also heard groans from my more technically-minded friends at its mention. I'd like for some amount of control over the code, but also for a lot of it to be pre-built for me.

A friend of mine recommneded that I use something called [Jekyll](https://jekyllrb.com/), which is what I ended up using for this website here. Jekyll describes itself as a simple way to make static pages, and as great for blogs. Sounds good! It lets me explore as deep as I want in HTML and CSS, whilst providing a simple framework to build static web pages and blog posts out of reusable layouts, includes, and plugins. The posts themselves are written in [Markdown](https://daringfireball.net/projects/markdown/), with a [YAML](https://yaml.org/) "front matter" block to set up variables like the post title, tags, etc..

Jekyll also has plug-and-play support for [Github Pages](https://pages.github.com/), so I don't need to worry about hosting, and can experiment to my heart's content without paying any costs. I found a theme I liked on Github and, given the open-source-ness, this was a great way to get my hands dirty with some HTML, SASS, and markdown. I chose the [Chirpy](https://chirpy.cotes.info/) theme as a starting point, and modified it a bit to suit my needs.

Another interesting tool that this introduced me to is Github Actions, Github's own continuous integration system, similar to Atlassian's Bamboo that I use in my current job. Chirpy already had a full build/test/deploy plan set up so I didn't need to explore this much, but I look forward to trying it out properly in the future. At this point I'm basically about to dive into five different new technologies at once, so I'm grateful to the author of Chirpy that I can draw the line here!

## Customisation

---

I wanted this website to do two things: show off my work as a portfolio, and host blog posts. Each work page should have a small blurb about what I've done, and some pictures or links to related blog posts. Meanwhile, blogs are to be more long-form posts that go into a bit more depth. I want to give them both some space on my homepage, as well as a directory page for each where every post can be found.

The theme I chose had a few useful things built-in such as post tags, side tabs, and post previews. Jekyll made it easy to identify work-posts and blog-posts by just adding a variable to each page's "front matter" block, and then I was able to edit some scripts to filter posts by that variable to build the homepage and work/blog directories. I can also add a thumbnail image in the same way, and I use a CSS flex element to display it next to the post previews.

Jekyll also has some pagination functionality; that is the ability to break a number of posts up into groups, so that I can show a sensible number of post previews on one page. Unfortunately, the pagination has a few limitations and, as such, I can add pagination to only one directory. I decided to opt for the blog directory over the work directory, as I expect to write more blogs than I will work jobs!

---

That about wraps up the work I needed to do, and changes I needed to make to get this website up and running. It doesn't seem like much at all, which says a lot about Jekyll and web technologies; they're a bit of a black-box, but they're powerful and designed to get this kind of thing up and running in a matter of hours. All-in-all I spent about 8 hours setting up my website, and now all that's left to do is to write the content!
