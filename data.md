# Processing the VCD file
What we are looking for is the time between the last character being sent on Channel 0 and he time first character is being sent on Channel 1. The stop bit is always high, so the last character will always be denoted in the file with `1!` and the start bit is always low so the first character sent on Channel 1 will be denoted with `0"`. So effectively we're looking for all the time differences between `1!` and `0"`.

Now, if you're feeling adventurous go ahead and write a script that will calculate these time differences. You have all the necessary information: file format and what we're looking for. So go. Try it. It's fun. I promise.

If you don't want to do it yourself, here's a Python script that does just that:

```
from __future__ import print_function
import sys

f = open(sys.argv[1])
last = False
timestamp = 0

for line in f:
    if line[0] == '$':                               # skip the preamble
        continue
    elif line[0] == '#':                             # it's a timestamp
        prev_timestamp = timestamp
        timestamp = int(line[1:])
    elif line[0] == '0' and line[1] == '"' and last: # it's the character we're interested in
        print(timestamp - prev_timestamp)            # just print it for now
        last = False
    elif line[0] == '1' and line[1] == '!':          # we will be interested in the next character
            last = True
```

This will print all the password check times in nanoseconds. The next step is to correlate the times with the byte values we've tried. The first 100 timestamps will be byte `0`, the next 100 will be byte `1` and so on. So, instead of printing the values we can save them in a dictionary keyed by the byte value. The print statement becomes:

```
timestamps.setdefault(byte, []).append(timestamp - prev_timestamp)
if len(timestamps[byte]) == 100:
    byte += 1
```

This puts all the timestamp differences into a dictionary and if the specific key already has 100 values we need to switch the byte value to the next one. Now all that's left is to calculate the average value and sort the values. Remember, we're looking for the byte value for which the average validation time is the longest.

```
for byte in timestamps:
    timestamps[byte] = 1.0 * sum(timestamps[byte]) / len(timestamps[byte])

ordered_keys = sorted(timestamps, key = timestamps.get)
for byte in ordered_keys:
    print("{} {} {}".format(timestamps[byte], byte, chr(byte)))
```

This code goes over every key and calculates the average value. Then it sorts all the keys by the values and prints them to the output. The output should look like this:

```
57300.0 138 �
57316.0 32  
57480.0 80 P
57480.0 99 c
58088.0 112 p
```

So, on average, the password starting with `p` took the longest time to validate. Is this really the first character of the password? Well, we can also imply that from the data we see. Let's take a look at the differences between the times it takes to validate different guesses:
T
Byte | Time to validate | Time difference 
-----|------------------|-----------------
112 | 58088 ns | ---
99  | 57480 ns | 608 ns
80  | 57480 ns | 0 ns
32  | 57316 ns | 164 ns
138 | 57300 ns | 16 ns

You can see that the values other than 121 (`p`) are clustered together more tightly and `p` is an outlier (it takes 608 ns more to validate it than to validate the second password guess on the list). This supports our assumption that `p` is the first character of the password. Well, this and that code we uploaded to Arduino Uno which specified that the password was `passw`...

Either way, we managed to get the first letter of the password. Now that we know the first one, we can repeat that process for the second letter (using `p` and the first and filling the last 3 charcters with zeros) and so on until we have the whole password.

The remaining question is: how does that speed up the password brute-forcing?

Brute-force: <img src="https://render.githubusercontent.com/render/math?math=256^5 = 1,099,511,627,776">

Timing attack: <img src="https://render.githubusercontent.com/render/math?math=(256 * 100) * 5 = 128,000">

So a timing attack is 8.5 million times faster. On Arduino Uno the whole attack from start until the end will take about 10 minutes.

In the next part we'll try to fix the `checkPass` method so that a timing attack is no longer feasible.

[Check every character of the password guess >>>>](power.md)
