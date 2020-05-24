# Frequently Asked Questions
Below are the questions that workshop participants ask most often.
## You said that the bitrate is 119,047 bps, but I've seen you type in 115,200 to the terminal. Why?
115,200 bps is one of the standard serial port bitrates. It's actually the bitrate that I've set up in the Arduino code. However, CPUs run on specific clocks, which may not be compatible with the bit rate. Let's say that the CPU runs 100 instructions per second and you want to send out 40 bits per second. This is not possible because 100 isn't divisible by 40. So the CPU will try to send out a number of bits as close to 40 as possible. In almost all circumstances it doesn't not matter that much.

## What's up wit that `waste` loop in the fault injection routine?
We cannot ue `delay` to calculate very short amounts of time, because of the same reason that is mentioned in the answer above. the `delay` method is based on time, not on CPU clock cycles so it tries to approximate the requested delay. In order to do that it has to perform some calculations and is very inaccurate if used to skip few CPU cycles. Having a for loop that does nothing is more reliable.

## Are these attacks really a problem?
If you have a modern computing device (laptop, mobile phone etc.) it probably has some kind of a secure chip which holds some kind of a password. This is in order to compartmentalise tasks and make your device more secure -- password is stored on a chip designed for password storage and not anywhere else.

This password should not be directly accessible, but rather it should only be possible to check if the user supplied password is correct or use that password for encryption. If you gain access to that chip (e.g. you steal the device) you may be able to perform one of the attacks and recover the password.

## Are there real world, practical examples of such attacks?
Yes, for example [Xbox 360 was hacked using a power glitch attack](http://www.logic-sunrise.com/news-341321-the-reset-glitch-hack-a-new-exploit-on-xbox-360-en.html).

## I want to try it on some hardware!
That's great! You should have all the code and the instructions covered in this workshop. Just buy two Arduino Unos, logic analyser, breadboard, some wires, resistors and transistors. It's not that expensive and you're going to have lots of fun.

## This is terrifying.
Don't worry, there are people who are researching IT security and making the world a safer place. Making that research universally accessible actually is useful for the overall state of the security, as it keeps everyone aware and alert.

## I still have questions.
Feel free to ask them on my Twitter: [@maldr0id](http://twitter.com/maldr0id). Thank you and enjoy your day!
