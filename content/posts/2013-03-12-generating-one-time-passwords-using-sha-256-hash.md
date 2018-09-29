---
author: Amy
categories:
- Projects
date: "2013-03-12T03:02:05Z"
tags:
- computer security
- cryptography
- GUI
- hash
- Java
- passwords
- Swing
title: Generating one-time passwords using the SHA-256 hash function
url: /posts/2013/03/generating-one-time-passwords-using-sha-256-hash/
---

A one-time password (OTP), in the realm of computer security, is a password that is only valid once and expires as soon as it is used. OTPs are important because they provide an added layer of security to many systems, especially since they are invulnerable to replay attacks.

My latest project was to make a Java program that could generate OTPs using the SHA-256 hash function. SHA, designed by the NSA, stands for Secure Hash Algorithm and serves to hash a dataset of any size into a fixed-size output (256 bits, in this case).

I incorporated GUI into my program to better simulate a real-life usage of OTPs. I used Swing (a GUI widget toolkit used with Java) to create a system that displayed two windows. The first window was simply for generating and displaying an OTP. The second window was for inputting an OTP and verifying if that OTP was valid.

The basic program design was to first generate a (psuedo)random number using Java&#8217;s SecureRandom class, which is cryptographically strong and far more secure than the typical util.Random class. This was provided as a seed to the SHA-256 hash function, and the producing hash was converted into a six-integer-long number (e.g. 123456), the OTP. This hash would then be passed in to the hash function for the next generation of an OTP, and so on.

So for example, say we are a bank and want to give our customer a temporary password so the customer can log in to their account with this password and reset their login info. The customer can click on the button in the first window to generate the password. (In reality, a distribution method such as sending a text with that password to the customer would probably be implemented, but let&#8217;s go with this simple illustration.) Now that the customer has the password, s/he can input the password into the input box in the second window, click the verification button, and the program for the second window will calculate the next OTP and compare that with the input. If it matches, the user is granted access &#8211; otherwise, the user is denied.

Fairly simple, but the crux lies in keeping both &#8220;windows&#8221; in sync. Each program calculates the next hash function output, and therefore OTP, separately and keeps track of how many times a password has been generated. So what happens when someone clicks on the generate password button twice and the verify password button only once? Well, the verification program is still expecting the first password. So in my program I created conditional branches for if the count in one of the programs is higher or lower than the other, and instructions to the lower-count program to go through enough password generation cycles to reach the count of the other program. This only applies to counts that are within 100 difference of each other, though &#8211; we don&#8217;t want an attacker doing this a whole bunch of times and analyzing what happens so they can find ways to break in.

If you are interested in viewing the code for this project, I have a sample up on Github: <a title="https://github.com/aschlesener/SHA-256HashOTP" href="https://github.com/aschlesener/SHA-256HashOTP" target="_blank">https://github.com/aschlesener/SHA-256HashOTP</a>

Stay tuned for an update that will include pictures of the GUI windows in action!