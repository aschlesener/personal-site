---
author: Amy
categories:
- Coding
- Electronics
date: "2016-05-16T00:07:22Z"
tags:
- arduino
- microcontrollers
title: Programming Microcontrollers with Arduino
url: /posts/2016/05/programming-microcontrollers-with-arduino/
---

Recently I went to a workshop on programming microcontrollers with Arduino, hosted by the awesome <a href="https://twitter.com/sehurlburt" target="_blank" rel="noopener">Stephanie Hurlburt</a>. My expertise being more in software than hardware, I was super excited to try it out.

Turns out it&#8217;s pretty easy to get started building a basic circuit &#8211; assuming you already have the hardware (microcontroller, breadboard, resistors, USB wire, any LEDs and other doohickeys you may fancy), you just download the right USB driver for your microcontroller and get the <a href="https://www.arduino.cc/en/Main/Software" target="_blank" rel="noopener">Arduino IDE</a>.

I found Ladyada.net&#8217;s great <a href="http://www.ladyada.net/learn/arduino/" target="_blank" rel="noopener">Arduino tutorial</a> to be very helpful for getting started. <a href="http://www.ladyada.net/learn/arduino/lesson1.html" target="_blank" rel="noopener">Lesson 1</a> explains how to configure the Arduino software to work with your particular chip, and how to upload sketches (which are the scripts used to program your microcontroller). <a href="http://www.ladyada.net/learn/arduino/lesson3.html" target="_blank" rel="noopener">Lesson 3</a> covers the electronics fundamentals behind setting up the breadboard. After that, it&#8217;s mostly a matter of learning how to build programs to handle various aspects of the microcontroller &#8211; working with the Serial library, using inputs and outputs, and of course making lights blink.

My workshop partner and I created a super simple program that mimics an absurdly fast stoplight. Basically it runs in an endless loop, making first the green light go on, then the yellow light (for a shorter amount of time), then the red light.

[<img class="alignnone size-medium wp-image-86" src="/wp-content/uploads/2016/05/20160515_1610022.jpg" alt="20160515_1610022" width="300" height="238" />](/wp-content/uploads/2016/05/20160515_1610022.jpg)

There&#8217;s a picture of the LED stoplight setup. Each LED has a resistor connecting it to a socket on the microcontroller &#8211; for example, the lit-up one in the picture, the top green one, is connected to the D4 socket.

So in the setup function in the sketch, the three LEDs are initialized as outputs, which looks like this:

    void setup()
    {
      pinMode(2, OUTPUT);
      pinMode(3, OUTPUT);
      pinMode(4, OUTPUT);
    }
    

Then there&#8217;s the loop function turns the LEDs on and off by manipulating the voltage level.

    void loop()
    {
      digitalWrite(4, HIGH); // This turns on green LED at pin 4
      delay(1000); // Leave the light on for a second
      digitalWrite(4, LOW); // This turns off green LED at pin 4
      digitalWrite(3, HIGH);
      delay(500);
      digitalWrite(3, LOW);
      digitalWrite(2, HIGH);
      delay(1000);
      digitalWrite(2, LOW);
    }

And that&#8217;s it! Plugging in the microcontroller and uploading that sketch causes the lights to blink in sequence. Yay!

[<img class="alignnone size-full wp-image-87" src="/wp-content/uploads/2016/05/trafficLightGif.gif" alt="trafficLightGif" width="427" height="240" />](/wp-content/uploads/2016/05/trafficLightGif.gif)

Next step for me is buying a ton of parts from <a href="https://www.adafruit.com/" target="_blank" rel="noopener">Adafruit</a> and experimenting to see what kinds of cool stuff I can create now.