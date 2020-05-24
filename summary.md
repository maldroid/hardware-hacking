# What have you learned?
During the course of this workshop you've learned:

* How the serial protocol works
* How to use logic analyser
* How to perform a timing attack
* How power traces allow you to infer what instructions are running on the CPU
* How fault injection lets you bypass the password authentication completely
* How to read bullet point lists with a summary of the workshop

## Timing attack
Timing attack is an example of a set of techniques called side channel analysis (SCA). Side channel analysis simply means that you're measuring some value of the environment in which the CPU is running (e.g. the time that passes) and infer something about the computation. In our case we were measuring how long it takes to validate a password and, based just on that value, we were able to extract the first letter of the password. There are other side channels that are very useful: electromagnetic radiation, power usage, temperature measurements, noise.

In fact you're probably using SCA every day! When the fan on your CPU starts running you're inferring something about the CPU itself: that it's probably doing intensive computations. You may even measure the temperature of the CPU and GPU and figure out which one has bigger load. This is an example of an SCA.

## Simple and differential power analysis
Looking at the power traces in order to figure out what computations are done is called *simple power analysis*. Since you're measuring the power draw of the CPU it's still an SCA attack. When we looked at the power usage to find the 5 iterations of the loop which validates the password we were just using our eyes to find something interesting - hence *simple* in *simple power analysis*.

However, when we were comparing two power traces and subtracting the values (or rather calculating the difference) we were using *differential* power analysis. Now, *differential* power analysis is a much more complex and powerful tool and uses statistical methods to infer something about the computation. It can even extract encryption keys for RSA or AES! Our example was a very simple one, but it also showed how powerful it is.

## Fault injection
When we were glitching the CPU by turning the power off for a very short amount of time we were using one of the techniques of the *side-channel attack* (still called SCA). The difference between an *attack* and the *analysis* is somewhat subtle, but important. *Analysis* means that you're not interfering with the CPU while it performs the computation. *Attack* means that you do (e.g. by turning off the power).

## How to protect against these attacks?
In almost all instances mitigating these kinds of the attacks requires a combination of both hardware and software techniques. For example if you want to protect against the fault injection you can check the password twice instead of once, mitigating the single point of failure. You can also add a second chip which monitors the voltage and if drops below a certain level, even for a very short amount of time, the chip will power the CPU down.

## Are the attacks important? You have to have access to the hardware, right?
Yes, you have to have access to the hardware.

So, have you ever left your laptop unattended in a hotel room?

That is all! **Thank you for following along and staying until the end!**

If you still have some questions left click below.

[Frequently Asked Questions (and answers to them) >>>>](faq)
