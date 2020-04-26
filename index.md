# Welcome!

This course shows basics of hardware in a completely hands-off way. You only need a computer running Linux, macOS or Windows. You do **NOT** need additional hardware or experience with hardware or hacking. All of the examples are based on Arduino Uno and if you have this board you can replicate the tasks, but there's no need for it. I promise you will still have a bit of fun. Before we start make sure to download and install the [Logic software from Saleae](https://www.saleae.com/downloads/) and make yourself a cup of tea (or any other beverage of your choice -- it doesn't need to come in a cup!).

## Password verification

Our task is simple. We have an Arduino Uno board with a 16MHz CPU. We assume that the memory (including the code running on the device) is inaccessible.

We have to write a password checking method which takes a string as an argument and returns `true` if the string matches a hardcoded 5 character password and `false` otherwise. The password is hardcoded instead of being hashed, but all the methods presented here are generic enough that they work in various configurations. 

Our first try would probably be something like this (`loop` in Arduino is similar to `main` in regular C):

```
String PASSWORD = "passw"

bool checkPass(String buffer) {
  for (int i = 0; i < PASSWORD.length(); i++) {
    if (buffer[i] != password[i]) {
      return false;
    }
  }
  return true;
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

The password is 5 characters long and CPU runs at 16 MHz, which makes brute-forcing infeasible. So this code, theoreticaly, should work just fine. However, this course is all about hardware hacking this brings us to the first question...

> *What's wrong with this code? How can having a physical access to the device make cracking the password easier?*

[See the answer on the next page >>>>](timing)
