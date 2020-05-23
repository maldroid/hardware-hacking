# Fixing the password check(?)
The easiest fix is to not optimise the for loop and just check every character of the password like so:

```
bool checkPass(String buffer) {
  bool result = true;
  for (int i = 0; i < PASSWORD.length(); i++) {
    if (buffer[i] != PASSWORD[i]) {
      result = false;
    }
  }
  return result;
}
```

Well, there's still a bi of problem. Now depending on how many password characters are false the check will take longer (the `result` variable has to be updated). So if we have one character correct and rest of them incorrect the password validation will be faster than if all of the characters are incorrect. However, can we really measure that very small difference? Trying to repeat the same procedure, but looking for the fastest password validation gives us very inconclusive results:

```
63370.0 16 
63358.5 229 �
63346.0 47 /
63314.0 17 
63268.5 0
```

Not only `p` isn't on the list, but the difference between different byte values are small and there's no one outlier value. So it means that the problem is solved, the password is secure! Well, not really.

Saleae logic analyser also has analog channels, which makes it a bit like an oscilloscope (don't yell at me, it's a basics course, I can make this simplification!). It means that some channels can be used not only to measure whether the voltage is "high" or "low" (aka binary) but also the actual value of the voltage. Why is this important?

Different CPU instructions have different power usage. Intuitively (again, this is simplification) changing two bits of a variable will produce a dofferent power drain than changing just one bit. Similarly setting the boolean value to false will produce a different power usage than not setting it (this should be pretty obvious). So maybe we can just record the power usage and see if it's any different when we try `xxxxx` and `pxxxx`? Let's do that!
