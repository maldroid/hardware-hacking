# Welcome!

This course shows basics of hardware in a completely hands-off way. You only need a computer running Linux, macOS or Windows. You do **NOT** need additional hardware or experience with hardware or hacking. All of the examples are based on Arduino Uno and if you have this board you can replicate the tasks, but there's no need for it. I promise you will still have a bit of fun.

Before we start make sure to download and install the [Logic 1 software from Saleae](https://support.saleae.com/logic-software/legacy-software/older-software-releases) and make yourself a cup of tea (or any other beverage of your choice -- it doesn't need to come in a cup!).

> Tip: some of the images may appear to be too small to read. If that's the case then simply right click on them and choose "open image in a new tab" -- the images are usually much larger than github makes you think!

## Password verification

Our task is simple. We have an Arduino Uno board with a 16MHz CPU. We assume that the memory (including the code running on the device) is inaccessible to the attacker.

We have to write a password checking method which takes a string as an argument and returns `true` if the string matches a hardcoded 5 character password and `false` otherwise. The password is hardcoded instead of being hashed, but all the methods presented here are generic enough that they work in various configurations. 

Our first try would probably be something like this (`loop` in Arduino is similar to `main` in regular C):

```
String PASSWORD = "passw";

bool checkPass(String buffer) {
  for (int i = 0; i < PASSWORD.length(); i++) {
    if (buffer[i] != PASSWORD[i]) {
      return false;
    }
  }
  return true;
}

void setup() {
  Serial.begin(115200);
}

void loop() {
  Serial.print("Password:");
  char pass[PASSWORD.length()];
  Serial.readBytes(pass, PASSWORD.length());
  bool correct = checkPass(pass);
  if (correct) {
    Serial.println("Password correct!");
    Serial.flush(); exit(0);
  } else {
    Serial.println("Incorrect password!");
  }
}
```

Let's take a look at the code and figure out what it does. First it prints a password prompt and then tries to read as many bytes as is the password length. Once it does that it runs the `checkPass` method. This method goes over the password byte by byte. If one of the bytes of the input doesn't match the actual password the method returns `false` and the `Incorrect password!` message is printed.

If, on the other hand, every byte of the input matches the password the `checkPass` method returns `true` and we get the `Password correct!` message.

The password is 5 characters long and CPU runs at 16 MHz, which makes brute-forcing infeasible, as it would take years. One password try takes approximately 62.5 microseconds. So in total we would get:

<img src="https://render.githubusercontent.com/render/math?math=256^5\times 62.5\mu s = 68,719,476.7s = 2.17 years">

If that doesn't convince you, brute-force is just plain boring. All in all this code, theoreticaly, should work just fine. However this course is all about hardware hacking, so this brings us to the first question...

What's wrong with this code from the hardware hacking perspective? How can having a physical access to the device make cracking the password easier?

[See the answer on the next page >>>>](timing)
