---
author: Amy
categories:
- Coding
- Educational
date: "2015-12-05T01:01:01Z"
tags:
- ef7
- entity framework
title: No laziness allowed with Entity Framework 7 beta
url: /posts/2015/12/no-laziness-allowed-with-entity-framework-7-beta/
---

If you&#8217;ve worked with .NET before, you&#8217;ve probably run across <a href="http://www.asp.net/entity-framework" target="_blank">Entity Framework</a> (EF) &#8211; a tool that essentially helps you map entities to data. For example, a project that I&#8217;ve been working on in order to get up to speed with the new .NET 5 and EF 7 beta releases is a simple recipe web app that lets you create, edit, and view recipes.

For the recipes to be saved somewhere, I&#8217;m using EF to map the recipe model properties to columns in a database table. It&#8217;s pretty easy to get started with EF; I hadn&#8217;t had much prior experience with it, but there are lots of great tutorials out there, including the official <a href="http://ef.readthedocs.io/en/latest/" target="_blank">documentation</a> for the beta release.

Unfortunately for those of us just getting started with EF, many of the tutorials based on older versions (which is most of them) no longer apply.

Many of these breaking changes are documented. However, I did run across a few issues that weren&#8217;t as easy to find.

One particular issue was that at first my recipe ingredients just weren&#8217;t being displayed on the recipe view page, even though the data was there and the code seemed right. I was simultaneously working through a <a href="http://www.asp.net/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application" target="_blank">EF6 tutorial</a> to learn the basics (you have to tweak a few things to get it to work with EF7, but most of it is still applicable), and noticed that the same issue was occurring even in the tutorial app.

After some Google-fu I came across <a href="https://github.com/aspnet/EntityFramework/issues/3312" target="_blank">a Github issue</a> pointing out that lazy loading is not supported in EF7.

In other words, if you expect related entities to be loaded, you need to explicitly state so by including them. In my recipe ingredient example, I had to add a line of code in the view function in my controller that looks like this:

`_context.Recipe.Include(b => b.RecipeIngredients).ToList();`

Aaaaand now my ingredients are showing, just like I expected them to originally.

Many of these older EF tutorials (which are, again, nearly all of them since EF7 is still in beta) rely on lazy loading. They don&#8217;t even tell you that there are different ways of loading related entities, or what those ways are (MSDN has a good, albeit outdated, <a href="https://msdn.microsoft.com/en-us/data/jj574232.aspx" target="_blank">article</a> about the different types). It&#8217;s just one of those issues that happens when you&#8217;re using a beta that doesn&#8217;t have full documentation yet, especially of breaking changes.

If you read through some of the <a href="https://github.com/aspnet/EntityFramework/issues/3797" target="_blank">other</a> Github issues that have been opened about the lack of lazy loading in EF7, some people consider this to be a really horrible change. But as other commentors pointed out, it&#8217;s also a matter of performance &#8211; lazy loading can be really bad performance-wise (do you really need all related entities automatically loaded in all your views?!), so it&#8217;s probably good to encourage the disuse of it.

That being said, for such a breaking change, some more visibility in terms of documentation would have been helpful, especially for newcomers. I didn&#8217;t find anything in the official documentation mentioning the dropping of support for lazy loading. I did, however, find another sort-of-breaking change mentioned in the docs, which is lack of support for <a href="http://ef.readthedocs.io/en/latest/modeling/relationships.html#many-to-many" target="_blank">many-to-many relationships</a>. This also caused an issue for me that was confusing since it worked just fine in the EF6 tutorial! I actually came across the issue while dealing with a slightly different one, which I <a href="http://stackoverflow.com/questions/34098247/in-entity-framework-with-mvc-how-to-create-related-entity-of-related-entity-at" target="_blank">posted</a> about on StackOverflow.

Anyway, I&#8217;m not sure whether or not lazy loading will eventually be included with EF7, but I can&#8217;t wait until it&#8217;s officially out of beta and there is full documentation! Although, it is really fun to follow along with a beta framework and watch all the changes it goes through.