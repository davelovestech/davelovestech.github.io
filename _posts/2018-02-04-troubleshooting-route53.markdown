---
layout: post
title:  "Troubleshooting Route 53"
date:   2018-02-04 22:42:40 -0700
categories:
---
I was excited to discover that www.bioinformaticsanalyst.com was not a registered domain name! I knew it'd be the perfect website for me for a couple of great reasons: I am interested in transitioning careers from biochemistry to bioinformatics. I have a bachelor's degree, so I don't qualify for bioinformatics scientist positions. However, I do qualify for most bioinformatics analyst positions. I've seen other computationally-intensive biological research jobs, like data wrangler, but I feel that "bioinformatics analyst" sounds more professional.  I decided to use Amazon Web Services for DNS routing and GitHub pages for website hosting because I see those services mentioned in job postings frequently. I've hosted sites through GoDaddy and HostGator, so I thought Route 53 would be easy, but I was wrong.
![Site Unavailable]({{"/assets/troubleshooting-route53/site_unavailable.png" | absolute_url }})
I was hoping to see my nice new website, but all I found was a broken link and a message that my website couldn't be reached. There are a lot of interconnected systems that all need to work together properly for website hosting, so I knew there was a problem somewhere in there. I decided to check the easiest thing first: the functionality of the code and files of my website. I'm using the Ruby tool Jekyll to convert Markdown files I've written into a static website. All I had to do to check my website files was to navigate to the directory of my website and run the linux command "bundle exec jekyll serve" to begin hosting the website on my personal computer at the address: http://127.0.0.1:4000/. As you can see from the screenshot below, the default website template was running perfectly well on my personal computer.
![Jekyll Welcome]({{"/assets/troubleshooting-route53/jekyll_welcome.png"}})
GitHub Pages and AWS Route 53 *initially* seemed intuitive enough to do without a protocol, but I clearly needed help figuring out what was wrong. That's why I started trying protocols that I found through Google searches. I tried following instructions from [OctoPerf], [James Hamann], and [Daniel Whyte]. The bad news was that I couldn't get my site to be hosted. But, the good news was that those protocols guided me through using AWS S3 website hosting **instead** of GitHub pages and my website was **still** down. This would suggest that GitHub Pages might not be the problem, so I checked my GitHub Pages settings and found GitHub Pages to be working perfectly.
![GitHub Site Ready]({{ "/assets/troubleshooting-route53/github_site_ready.png" | absolute_url }})
Additionally, I checked the GitHub local version of my website at davehalvorsen.github.io and that worked as well. That's when I realized it had to be on the AWS side, so I did some extra research. Route 53 is a Domain Name System (DNS) that means that it converts internet user requests for human-readable websites, like Google.com, into the computer IP addresses that the data is actually stored at. Amazon uses four different name servers per web domain to ensure that at least one server will be functioning if the others fail. While researching Route 53 I found a [video of an Amazon Engineer] explaining the possible causes of the problem I was experiencing.

The four name servers of my domain (www.bioinformaticsanalyst.com) **did not** match the four name servers of my Hosted Zone on Route 53. Copying the name server records from my Hosted Zone into my domain name solved the problem. I've since realized that I encountered this whole issue because I'd deleted my first Route 53 Hosted Zone and reinstated it without equating the values to my domain's name servers (because new values are assigned each time). I hadn't been following someone's instructions, so I was creating & destroying Hosted Zones to get a feel for how Route 53 worked, but I inadvertantly caused myself hours of unnecessary troubleshooting. Sure, I wasted a lot of time because I wasn't following a protocol, **BUT** my disorganized tinkering gave me an opportunity to learn more about the internet and also continue to build my troubleshooting skills. As you can see, I got the site working! Here are those *correct* DNS values:
![Record Set]({{ "/assets/troubleshooting-route53/record_set.png" | absolute_url }})







[octoperf]: https://octoperf.com/blog/2015/06/01/host-jekyll-on-s3-cloudfront/
[james hamann]: https://medium.com/@jameshamann/migrating-your-jekyll-website-to-aws-bc9582b3fbb2
[daniel whyte]: http://danielwhyte.com/app/design/2014/10/05/creating-a-jekyll-s3-server.html
[video of an Amazon Engineer]: https://aws.amazon.com/premiumsupport/knowledge-center/route-53-dns-website-unreachable/    
