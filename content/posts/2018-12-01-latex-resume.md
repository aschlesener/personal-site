---
title: "Creating a Resume in LaTeX"
date: 2018-12-01
categories:
- Coding
- Educational
tags:
- latex
- resume
- design
url: /posts/2018/12/latex-resume/
---

#### If you've ever been frustrated by formatting or design issues in Microsoft Word/Google Docs, welcome to the beauty that is [LaTeX](https://www.latex-project.org/).

### Background

I used LaTeX back in my college days for writing computer science papers, and after getting over the inital learning curve fell in love with it. Being able to programatically and logically lay out content just makes so much sense. But post-academia, I hadn't found much use for LaTeX.

Over the years, my process for updating my resume had gotten annoying; I had to make sure I was on a device that had Microsoft Word installed, then do tedious work to make updates to my custom table layout, then upload the resume to my Google Drive so I could access it from any device. Google Docs was available cross-device, but even worse in terms of design.

So when I recently went to update my resume, I thought back to how much I used to love LaTeX and figured I'd give it a shot again, this time for my resume. Here's how I did it.

### Using LaTeX to Write Your Resume

1. The first step is to brush up on some of the basic concepts of LaTeX. If you're new to it, I recommend checking out Overleaf's [Learn LaTeX in 30 Minutes](https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes) guide. I'll talk about Overleaf more in a minute, but I actually found them through their excellent LaTeX documentation, so this is a great resource to start with.

2. Next, you'll probably want to find a resume template to build off of, rather than start from scratch. I just googled "latex resume" and found my starting template that way. You can also try out [my template](https://github.com/aschlesener/resume/blob/master/AmySchlesenerResume.tex).

3. Once you have a .tex file, you'll want to edit it with your own content and make design tweaks as you see fit. You can either: 1. install LaTeX on your machine, or 2. use an online service to edit/preview your changes.  
  1. Depending on how you choose to install LaTeX (see the options [here](https://www.latex-project.org/get/)), if you want to convert to PDF you will either need to do it via command line (`pdflatex yourresume.tex`) or use a built-in GUI.  
  I chose not to go down this route, because I want the ability to edit and preview my resume on any device without needing LaTeX installed.
  2. My recommendation for an online service is the site I mentioned before, Overleaf. [Overleaf](https://www.overleaf.com?r=cdd519b3&rm=d&rs=b) is an online service that lets you write your LaTeX document with syntax checking and auto-compilation, and provides a preview of the generated PDF. It also has other fancy premium features like collaboration and syncing with Github, but the free version is sufficient for testing out resume changes.  
  (Note: If you click the  referral link above and sign up for Overleaf, you will help me unlock some premium features for my account. If you want to be stingy, you can also go straight to https://www.overleaf.com.)

4. Admire your shiny new resume!

If you run into any issues converting your resume to LaTeX, or have any general LaTeX questions, feel free to reach out to me over email or Twitter.

### Links
* Live preview: https://www.amyschlesener.com/docs/AmySchlesenerResume.pdf
* Repo including LaTeX source file: https://github.com/aschlesener/resume
