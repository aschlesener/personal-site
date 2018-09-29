---
author: Amy
categories:
- Educational
date: "2013-11-10T01:34:43Z"
title: The magic of PHPMailer
url: /posts/2013/11/the-magic-of-phpmailer/
---

Recently I updated the website for my club, the <a title="AWC" href="http://awc.cs.wwu.edu/" target="_blank">Association for Women in Computing at WWU</a>, to send e-mails via <a href="http://phpmailer.worxware.com/" target="_blank">PHPMailer</a>. We have a contact form with the usual subject, content, name, etc. and want to send all that to our e-mail address. The previous e-mail system went through the school&#8217;s network  and was very much broken, so it was in need of a good fix.

It was easier than I thought it would be, mostly just building some basic PHPMailer code. Add the script to your site, create a new instance of PHPMailer, and set the various properties on that instance. For example, if your mailer is $mail, you set the subject of the email like so:

`$mail->Subject ="E-mail subject here";`

Then once you&#8217;re done adding all the properties, you do `$mail->Send`, and you&#8217;re good to go!

For the server, I ended up just creating a new Gmail e-mail address so I could use Gmail as a host. So our host property looked something like this:

`$mail->Host = "smtp.gmail.com";`

And then you use the Gmail address and login for the Username and Password properties. Simple and easy!

PHP does already have a built-in mail function, conveniently called mail(), which is fine for super simple e-mails but PHPMailer makes it easy to do more complicated things, like adding an attachment or using SMTP to send the e-mail. So unless you only want to do very basic things with mail in PHP, I&#8217;d stick with a library.

Official instructions and examples can be found on their <a title="PHPMailer" href="https://github.com/PHPMailer/PHPMailer/blob/master/README.md" target="_blank">Github</a>.