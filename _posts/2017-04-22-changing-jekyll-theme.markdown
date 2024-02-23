---
layout: post
title:  "Index.md Does Not Exist!?!"
date:   2017-04-22 22:42:40 -0700
categories: Troubleshooting
---
Manually coding the CSS and HTML for creating a website is a slow and repetitive process. I completed the [Codecademy Web Development Program] and I *highly* recommend it for anyone interested in creating websites from scratch. *But*, I already know HTML & CSS and there are plenty of code examples on my GitHub already. I just want to write articles about my career transition without having to pause and contemplate syntax. That's one of many reasons that I have chosen to use Jekyll and GitHub Pages for maintaining my website's content. Jekyll is a Ruby-based tool that takes simplified HTML, called Markdown, as input and then creates a fully-finished static website, ready to be hosted.

It replaces the previously complicated task of changing a website's themes with single line changes in centralized files. And, best of all, you can use Markdown to create HTML documents from RStudio and Jupyter Notebooks! Jekyll is the default static site generator for GitHub Pages, which is another fantastic reason to blog with Jekyll. Your Github, and your Github Pages account, can have code "pushed" to it directly from the terminal by using Git. Not only does this make blogging and coding more efficient, it's also a great way to get a new coder like me get into good habits for version control! **I've talked a lot about Jekyll being great, but that's not what this article is about.**  

I've been trying to remove the "subscribe via RSS" component of Jekyll's default theme all day, but I just can't seem to figure it out. Alternatively, I've been trying to change the default theme from Minima to a theme without an RSS feed, but I can't get that to work either. **I'm writing this article to force myself to think through where the problem might be, and also to prepare the technical support question I might need to post on the Jekyll GitHub page.** I'll be guiding you through my attempts at fixing the problem, the troubleshooting tips that others provided, and finally I'll show you how I got everything working the way that I wanted.

![Minima Theme Works]({{"/assets/jekyll_theme/jekyll_welcome.png"}})

The process for changing a theme for a website seems simple according to the [Jekyll documentation]. My OS is Ubuntu Linux, so this is what I need to do('$' signifies terminal inputs):
1. $ jekyll new NAME-OF-WEBSITE-HERE
2. Find a [new Jekyll theme] you want. Add "gem 'NAME-OF-DESIRED-JEKYLL-THEME'" to your site's Gemfile.
3. $ bundle install
4. Add "theme: 'NAME-OF-DESIRED-JEKYLL-THEME'" to your site's _config.yml file.
5. $ bundle exec jekyll serve  
I thought that's all I had to do to make this work. I tried changing my theme to Architect, but then I saw the blank main page of my site:

![blank site]({{"/assets/jekyll_theme/home_not_exist.png"}})

The peculiar thing about this seemingly-empty site is that it *does* have the right theme, *but* that theme appears to only be available for the posts (if you enter their exact address in) and also the 404 page. So the theme is working, but this new theme has broken my homepage. I tried a handful of themes, making sure they were [supported by GitHub Pages], and I only seemed to replicate that same initial problem of a broken homepage. Technically, I could push the Minima theme version of my blog to GitHub pages and then change the theme in GitHub pages, but I feel that would defeat the purpose of using Jekyll and GitHub pages. I'm trying to save time and learn version control, so I want to stick with the terminal and Makrdown revisions for running this site. Digging deeper, I see the first potential solution to the problem:   

![RubyGems Install Not Permissible]({{"/assets/jekyll_theme/theme_no_permission.png"}})

"Your user account isn't allowed to install to the system RubyGems." That kindof makes sense because RubyGems are supposed to be hidden from the user, but why would I encounter this error while following the developers' instructions? The simplest explanation is a mistake I made while following the directions, so I tried multiple times (with complete restarts) to ensure I was following the right steps, but the homepage still wouldn't work. The next most likely issue is with my software being up-to-date. I'm using ruby version 2.3.1 and Jekyll 3.7.3, which should be fine. But, take a look at that weird error message for the "$ jekyll -v" command!

![Jekyll Unresolved Specs]({{"/assets/jekyll_theme/theme_unresolved_specs.png"}})

I searched for this error message and I couldn't find anything relevant, so it's possible that this problem is more complicated than I am skilled. But, that's not the only weird error message! So, it's important for me to keep documenting. "$ bundle install" runs in the terminal as if it's going to work, but then it stops processing and asks for me to sudo in or to add a new argument to the end of the "$ bundle install" command. I tried to sudo in, but every time it just kept serving that same initial suggestion to sudo in. That's when I control-c'd out of "$ bundle install" and ran the "$ bundle install --path vendor/bundle". That last terminal command ran fine, but then the next command of "$ jekyll build" yielded that same "Unresolved specs" error that I got when I first ran "$ jekyll -v". Despite the weird error, it did report that the bundle was completed, so I continued by entering "$ jekyll bundle exec serve". And that's when I saw yet another error:

![index.md does not exist]({{"/assets/jekyll_theme/page_not_exist.png"}})

The layouts for my 'post', 'page' and 'home' files were not found to exist. I switched back to the functioning Minima theme and all of the problems went away, so I knew the error was linked to a theme change and not some other unrelated bit of code that I had accidentally changed while altering the theme. That's when I hopped on Google and searched for “Build Warning: Layout 'home' requested in index.md does not exist.” I found a [suggested solution] to my problem of adding a "_layouts" directory and files within it titled "post.html", "page.html" and "home.html." Each one of those files need to have a [YAML header and a chosen layout]. I tried "default", "single" and "post" chosen layouts and I also tried changing the themes, but the homepage maintained blank. I also checked the silly little things like that I was running the serve command from the right directory and that I'm using the right link to view my in-machine served Jekyll website.

I tried all of the methods I could think of to get my Jekyll theme to change and I couldn't manage it. I *thought* I was done with error collection, so I decided to post the Minima version of my blog as-is and then fork it for a version where I've attempted the theme change. My thinking on this was that I could point people to the exact codebases I was talking about without worrying about the size limitations of a forum post for my upcoming help request. BUT, my as-is site wouldn't load @bioinformaticsanalyst.com long after the files had synced, so I did some searching and discovered **another weird error message!**

![Vendor Bundle Fail]({{"/assets/jekyll_theme/vendor_bundle_fail.png"}})

I'm told that I have Markdown files with an incorrect date format, *BUT* I've compared against the [Jekyll date format documentation], so I know that my dates are correct ... weird. That's when I saw the location that the error message was talking about, "vendor/bundle", and I remembered that it was the location of the weird command that the terminal had asked me to enter (“>bundle install –path vendor/bundle”). Then I noticed that the as-is site had a couple thousand files! Why? I then ran an entirely fresh Jekyll build and found only 14 files total. Where did all the extra files come from for the Jekyll build with a theme change? Are one (many) of those new files responsible for the failure to compile the site on GitHub? It's odd cause the site compiles and self-hosts fine on my desktop, but then it can't compile on GitHub pages. Not surprisingly, I dropped working on the Jekyll theme change to focus on other projects. Two days later I finally figured it out!

I thought more about the _layouts folder and those file changes that were part of the [suggested solution] and also about the [YAML header and a chosen layout]. The suggestions to have a YAML header and content, within double curly braces, seemed to simple to be doing anything specifically, so I decided to play around with those files some more. I discovered that each Jekyll layout had a [specific format] that *usually* limits the type of posts you can make to the site. For example, the Minimal theme (not to be confused with my current Minima theme) **does not** allow posts to be of the format 'post'; the layout has to be 'single' to work. I re-read the [Jekyll documentation] and realized that I had misunderstood the instructions about adding a copy of the default.html file to the _layouts folder.

I didn't just need to copy default.html to _layouts ... if I wanted to change the default theme, I needed to copy *all* of the layout files into _layouts. For the [Minima theme], that means copying 'default.html', 'home.html', 'page.html', and 'post.html' into the _layouts folder. I think my issue arose because Jekyll saw 'default.html' in _layouts and decided I didn't want to use the default 'home.html' because I hadn't copied that file over into _layouts. Copying all of the defaults into the _layouts folder allowed me to introduce a new 'default.html' file, BUT I still had the "subscribe via RSS" text because I hadn't yet changed the new default.html from the initial file. So far, all I'd done was have a new 'default.html' file *without* losing my homepage. Looking through 'home.html', I had no trouble finding the line of code responsible for "subscribe via RSS":

![Delete This Line]({{"/assets/jekyll_theme/delete_this_line.png"}})

That **FINALLY** got rid of the "subscribe via RSS" from my website! You can see it worked by checking out my homepage. I've been trying to get my theme changed for *three* days. Initially it seemed like *such* an easy fix, but it turned out to be a monstrous task *because* I know very little about computer science. I'm perfectly happy that I had such a hard time figuring this out *because* everything that I learned will be applicable to the more complicated computational challenges that I'll be facing in the future. It seems I'm not alone in having trouble with the default Minima RSS feed. There's a whole [Github thread] dedicated to people asking for the RSS feed to *not* be included as a default because the Minima theme is supposed to be minima-l. I agree witht he posters from that link. Jekyll is supposed to be friendly to new coders without HTML/CSS experience, yet I had a *tremendous* difficulty figuring it out (and I've already studied a lot of HTML/CSS). All in all, I still love Jekyll and I'm excited about all of the ways that my site will improve in the future!


[Codecademy Web Development Program]: https://www.codecademy.com/
[Jekyll documentation]: https://jekyllrb.com/docs/themes/
[new Jekyll theme]: https://rubygems.org/search?utf8=✓&query=jekyll-theme
[supported by GitHub Pages]: https://pages.github.com/themes/
[suggested solution]: https://github.com/benbalter/wordpress-to-jekyll-exporter/issues/37
[YAML header and a chosen layout]:https://github.com/github/pages-gem/issues/416
[Jekyll date format documentation]:https://jekyllrb.com/docs/posts/
[specific format]: https://github.com/github/pages-gem/issues/416
[Minima theme]:https://github.com/jekyll/minima
[Github thread]:https://github.com/jekyll/minima/issues/98
