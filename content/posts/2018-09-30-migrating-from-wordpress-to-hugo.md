---
categories:
- Coding
- Educational
date: 2018-09-30
tags:
- golang
- aws
- hugo
- website
title: Migrating from Wordpress to Hugo - Why You Should Use a Static Site Generator
url: /posts/2018/09/migrating-from-wordpress-to-hugo/
preview: blah
---

## Table of contents
1. [Background](#background)
    1. [Hosting Wordpress on AWS](#hosting-wordpress-on-aws)
2. [Static Site Generators](#static-site-generators)
3. [Migrating from Wordpress to Hugo](#migrating-from-wordpress-to-hugo)
    1. [Customizing Your Hugo Site](#customizing-your-hugo-site)
4. [Deployment](#deployment)
    1. [Custom Domain Name](#custom-domain-name)
    2. [SSL](#ssl)
5. [Next Steps](#next-steps)


## Background
I've been running my personal site with Namecheap and Wordpress for the past 6 years. My site had a hand-coded static HTML and CSS site with standard pages like "about me", and a subdomain which hosted my Wordpress blog. \
It worked fine, but any site updates required dealing with cPanel. Writing blog posts was easy in the sense that Wordpress gives you a GUI to do so, but formatting was annoying and there's always the security risks that come with having a database and plugins.

### Hosting Wordpress on AWS
My first instinct when deciding to modernize my site was to just migrate both my static site and Wordpress blog to AWS, since I've been using AWS a lot at work and not having to deal with cPanel would  be a win. \
The static site is easy, just host it in a S3 bucket; but the Wordpress blog is more complicated, as it requires a MySQL database and a server.

There are numerous options to host a Wordpress blog in AWS, many of which are overly complicated for a personal site that doesn't get much traffic. The standard solution is to host it on an EC2 instance with a LAMP web server. As you can see from Amazon's tutorials, it takes a bit of [poking around](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-LAMP.html) to get everything set up properly. It can also get costly quickly.

Amazon's comprehensive [white paper](http://d0.awsstatic.com/whitepapers/wordpress-best-practices-on-aws.pdf) on hosting a Wordpress blog on AWS led me to what would have been the best solution for my case, which is to use [Lightsail](https://aws.amazon.com/lightsail/). Lightsail is basically Amazon's dumbed-down version of AWS that gives you a virtual private server with little configuration needed. It's also extremely cost-effective for light use - $3.50 a month for their most basic plan. 

Doing some back-of-envelope calculations, the cost of migrating my site to AWS (including the domain and static site hosting) was about $52 per year, which is slightly less expensive than the $60/year I was spending with Namecheap. Note that this doesn't include the cost of a private email address for my domain, which I get for free with Namecheap.

So the infrastructure would be modernized, but the cost would be about the same. And I would still have to deal with Wordpress.

## Static Site Generators
Looking into alternatives for Wordpress, I turned to static site generators. These have been popular for a while now among developers who blog. Basically you write your blog posts in Markdown, and use some library, often in conjunction with a pre-made theme using HTML/CSS/JS, to build your markup so it can be rendered client-side. \
It's a little more involved to write blog posts than Wordpress, since instead of using a WYSIWYG editor you write with Markdown. But I prefer building my posts in a way that I know how it's going to render, instead of a fuzzy text editor that mangles characters.

There are huge advantages to static sites over ones that require server-side functionality, a DB, and CMS. Obviously if you need the rich functionality of a CMS than you may want to stick with Wordpress, but for most people's personal blogs, the security issues, constant updates required, cost, and hassle associated with a CMS make a static site look very appealing. Remember, it's cheap and easy to stick a static site on something like an S3 bucket; it's much more complicated to deploy and host a full-blown "traditional" site.

The most popular static site generator is [Jekyll](https://jekyllrb.com/), often deployed for free with Github Pages. I've used this approach at work, but it forces you to to have a Ruby ecosystem installed which is not ideal unless you're already a Ruby dev. 

Another popular one is [Hugo](https://gohugo.io/). This one intrigued me, because while it's written in Go, it installs as a single binary. Meaning you don't have to have any kind of environment setup to use it - just download and run the binary. It's also crazy fast to build and generate sites. 

## Migrating from Wordpress to Hugo
Okay, so Hugo seems like an awesome alternative. But what about all the posts that were on my old Wordpress blog? Turns out it's super easy to migrate them! Here are the steps I followed ([this](https://gomakethings.com/migrating-from-wordpress-to-hugo/) is also a great resource, as it's basically the same steps that I did):

1. Install [Hugo](https://gohugo.io/getting-started/installing/). For Ubuntu, I just downloaded the latest binary from https://github.com/gohugoio/hugo/releases and added it to my bin with `sudo mv hugo /usr/local/bin`.
2. Download the [Jekyll exporter](https://wordpress.org/plugins/jekyll-exporter/) plugin.
3. Follow the plugin instructions to install it on your Wordpress blog and export your public posts. This will give you a zip with the Jekyll-converted versions of all your posts, which you should then download and extract.
4. Use the built-in [Hugo Jekyll importer](https://gohugo.io/commands/hugo_import_jekyll/) to convert the Jekyll posts from step 3 into a new Hugo site with Hugo blog posts.
5. Cleanup the migrated posts and image links as necessary.

That's it, you should now have all your old Wordpress blog posts in Hugo!

### Customizing Your Hugo Site
Remember how I said my old site had a static site that was separate from my Wordpress blog? At this point, I realized that instead of trying to simultaneously have that site and the new Hugo blog site, it made a lot more sense to just merge that static site into this new Hugo site. Especially since my old site was really just a landing page, an "About Me", and a link to my resume. So I tweaked the Hugo blog site to have similar colors and layout to my old static site, and migrated a few things from the old site (a JavaScript snippet to automatically update the copyright year, some text, and the general look and feel). 

You'll probably want to install a [theme](https://themes.gohugo.io/) for your new site. You can try to find one that looks like your Wordpress blog did, or just start fresh. Best practices are to install the theme as a Git submodule so you don't check in all the theme code. For overriding the theme, the properties you want to override, such as background color, are easy to set in your Hugo config file.

For more complicated overrides, like changing templating or layouts, you can add a new file in your Hugo site that will override the theme. For example, I wanted to add a preview snippet to each blog post in the list of posts, so I found the file with the relevent code in the theme I used (`list-item-post.html`) and copied the file to the same directory structure in my Hugo site, `layouts/partials`. Then I updated the code to include `{{ .Summary }}` where I wanted the blog post preview to show up. Hugo  uses Go templates, so when modifying your site you need to have basic knowledge of Go templating and what variables are available within Hugo. That's all pretty Google-able stuff though, so don't worry if you aren't familiar with either.

When testing changes locally, just run `hugo server -D` and by default your Hugo site will be available at http://localhost:1313. Hugo watches for changes and automatically reloads for you.

## Deployment
Earlier I mentioned deploying a static site to an S3 bucket. But after spending several hours migrating from Wordpress to Hugo and making the site look good, I wanted a fast way to deploy my fancy new site, with SSL, and automatically deploy any changes in my Github repo.

That's all doable with AWS, but takes some time. I just wanted to get my beautiful new creation out into the world as quickly as possible.

Enter [Netlify](https://www.netlify.com/).

Netlify gives you automated deployments of static sites, with SSL, with a click of a button. For free.

I literally just signed up, connected my Github, specified that it was a Hugo-generated site, and clicked Deploy. Within seconds my site was live to the world and would re-deploy on any Github changes to the master branch.

The only tasks left to complete the site migration were to point my amyschlesener.com domain name to the new Netlify-hosted site, and add SSL.

### Custom Domain Name
This was pretty easy - I added a CNAME to my domain in Namecheap, and had it point to the Netlify URL for my site. Once the DNS records propagated (maybe 10 minutes?), hitting www.amyschlesener.com loaded the new site.

### SSL
Since my previous site didn't have SSL, I never bothered to get a certificate. No problem - Netlify provides a free one for you via [Let's Encrypt](https://letsencrypt.org/).

However, I had problems getting this to work with my custom domain. I ended up switching my DNS zone to be managed to Netlify, which meant configuration changes in Namecheap and cPanel to allow that. 

After changing my domain's nameservers in Namecheap to point to the custom hostnames assigned to my DNS zone in Netlify, I kept running into issues getting the SSL cert to work, because there were still A records in the old DNS zone in cPanel. After deleting all those, it took a few hours for all the DNS changes to propagate and the SSL cert to be applied to my account.

Note that my domain email account provided through Namecheap did break somewhere during this process of switching the DNS zone to Netlify - i.e. incoming emails don't reach the account, despite adding the MX records from Namecheap to Netlify. For me it's not a pressing issue to solve, since most emails to that account are spam or recruiters, but it's something to keep in mind if you go down the same route.

**Update**: Shortly after this blog post was published, I solved the email account hosting issue by using [Migadu](https://www.migadu.com). They offer free email hosting with MFA and spam/virus protection, plus easy instructions for custom DNS setup. Adding the various DNS records (MX, SPF, DKIM, and DMARC) to Netlify was super easy, and after waiting several hours or so for the changes to propagate, my email was back up.

## Next Steps
I accomplished several things by switching from Wordpress to Hugo:

1. My hosting costs are much cheaper (I can now use the $48/year that I was spending on website hosting through Namecheap on better things, like more games on Steam).
2. My site is more secure, since you can't exploit a plugin or hack a database if there are no plugins or databases.
3. Updating my site and blog posts is a much more joyous experience - as a dev, I prefer to use my everyday tools like Markdown, a code editor, and Git over a bloated GUI.
4. My blog is faster. Wordpress posts would load sometimes painfully slowly; now that it's just an HTML page without bloat, posts load quite quickly.
5. Migrating to a new solution (for example a different static site generator) in the future will be easier, since my blog posts are now plain text and standardized in Markdown format.

But I still have my domain in Namecheap, so I need to migrate that somewhere else - and find a way to replace the free email account that came with Namecheap. No shade to Namecheap, they've been adequate for the last six years, but I would prefer using tools that are more developer-focused rather than dealing with cPanel. Also Namecheap has had this weird bug for the last year where trying to login with MFA leads to endless redirects for my account.  ¯\\_(ツ)_/¯

I'm also too closely coupled with Netlify. I love how easy (and free!) it is to have them take care of the infrastucture and automated continous deployment. But I like having more fine-grained control over components that I own, and I want a more robust solution in case Netlify were to go down or disappear.

I have a pretty cool solution in mind that should solve these problems, but I'll save that for the next blog post. ;) 

Hopefully this write-up of my experience in modernizing my site is helpful to you. If you're using a CMS like Wordpress or another static site generator like Jekyll, I highly recommend switching to Hugo - the binary installation and blazing speed is amazing. And give Netlify a try for an easy deployment solution. Overall, don't be scared of migrating an outdated personal site or blog to a new format - it can be fun and relatively quick, and a great learning experience.
