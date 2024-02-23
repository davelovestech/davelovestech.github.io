---
layout: post
title:  "Blogging with Jekyll & GitHub Pages"
date:   2017-2-28 11:58:40 -0700
categories: 
---
If you want to learn how to build a website, I *highly* recommend the [Codecademy Web Development Courses]. *BUT*, it'll take a long time to learn HTML & CSS (if that's all you plan to learn) and then it's *incredibly* boring and time-intensive to blog in those languages. That's why I chose to build my blog with the Ruby plugin [Jekyll] and host it on [GitHub Pages]. As long as you know your way around a terminal, you can write simple blog posts in [Markdown] and have Jekyll convert those into blog posts! In fact, I made this whole page in a few minutes using these techniques :) First, you'll want to install Jekyll. I use Ubuntu Linux, so I followed the [tutorial located here].

This is what installing Jekyll looked like on my system:
![Install Jekyll Dependencies]({{"/assets/blog_with_jekyll/install_jekyll_dependencies.jpg"}})

This actually isn't my first time blogging with Jekyll. I have had my personal blog hosted on GitHub Pages for a few months now. However, I've never taken advantage of using Git to work on my website from more than one computer. As you can see in this next terminal block, I fetched my website from my GitHub account, so that I can work on it on this new computer:
![Fetch Jekyll Website]({{"/assets/blog_with_jekyll/git_clone_website_jekyll.jpg"}})

Hosting the website on your personal computer for testing is another simple terminal command:
![Jekyll Serve]({{"/assets/blog_with_jekyll/bundle_exec_jekyll_serve.jpg"}})

Now all I have to do is navigate to my local server address and here's my website:
![Locally Hosted Website]({{"/assets/blog_with_jekyll/host_at_4000.jpg"}})

I really do love Jekyll! It makes blogging a breeze and I am very excited to start sharing more examples of my coding journey on this site! If you're thinking about learning to make websites, and you've got the time, I think it's worth learning the finer details of HTML and CSS. But, if you just want to start a good-looking blog without having to deal with Wordpress, then Jekyll is the plugin to use. My favorite aspect of Jekyll is that it's the preferred static-site generator for GitHub pages. I'm getting plenty of practice with GitHub even when I'm not coding. It's the perfect way to build your skills as a programmer!


[Codecademy Web Development Courses]:https://www.codecademy.com/catalog/subject/web-development
[Jekyll]:https://jekyllrb.com/
[GitHub Pages]:https://pages.github.com
[Markdown]:https://help.github.com/articles/basic-writing-and-formatting-syntax/
[tutorial located here]:https://jekyllrb.com/docs/installation/#ubuntu
