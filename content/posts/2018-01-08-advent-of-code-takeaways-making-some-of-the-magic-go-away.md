---
author: Amy
categories:
- Coding
- Musings
date: "2018-01-08T01:46:14Z"
tags:
- advent-of-code
- advent-of-code-2017
- golang
title: Advent of Code Takeaways - Making (some of) the Magic Go Away
url: /posts/2018/01/advent-of-code-takeaways-making-some-of-the-magic-go-away/
---

I did the Advent of Code in Golang <a href="/posts/2017/12/advent-of-code-2017/" target="_blank" rel="noopener">last month</a> and overall found it to be an excellent experience. The quality of the problems was good, although some days were too trivial, and it was a great learning experience for getting more familiar with a language &#8211; in my case, Go.

(I did about half of the advent before life happened, but you can check out the solutions to the first half <a href="https://github.com/aschlesener/advent-of-code-2017" target="_blank" rel="noopener">here</a>!)

One of the main takeaways it left me with was how ill-suited Go is in some ways for solving code problems like these. Parsing input and the lack of built-in functions that are standard in other languages like Python wore on me pretty quickly. For example, Go&#8217;s STL doesn&#8217;t have a lot in the way of collections; if you want a queue, you have to either write your own, or use somebody else&#8217;s from Github. I also found myself missing list functions, especially coming from C#.

Rob Pike, one of the creators of Go, gave <a href="https://talks.golang.org/2015/simplicity-is-complicated.slide#14" target="_blank" rel="noopener">a talk</a> about the simplicity of Go that notes the lack of features compared to other languages. Pike points out that Go was designed to be a &#8220;clean procedural language designed for scalable cloud software&#8221;, and as such was built with a minimal set of features to accomplish that goal.

&#8220;Uncle Bob&#8221;, or Robert C. Martin, of _Agile Software Development_ and _Clean Code_ fame, argued against the reckless use of overly-complex frameworks and languages in &#8220;<a href="https://8thlight.com/blog/uncle-bob/2015/08/06/let-the-magic-die.html" target="_blank" rel="noopener">Make the Magic Go Away</a>&#8220;. His view seems to be similar to that of the designers of Go.

>  Perhaps you should just write the little bit of code that you need, instead of importing thousands and thousands of lines into your project. Lines that you didn&#8217;t write. Lines that you don&#8217;t control. Lines that you probably shouldn&#8217;t be putting a whole lot of trust in.

Now, I have a computer science education. I can certainly write my own libraries if needed. And I love avoiding bloat in a codebase. That being said, spending time reinventing the wheel is not always the best option either.

As with any other language, Go is simply a tool. It&#8217;s not going to be the best language for every situation; it&#8217;s awesome for concurrent tasks and I know a lot of SREs who love it for their work. But for next year&#8217;s Advent of Code, I will likely be solving the problems using a language more suited to that kind of problem-solving.