# Limiting the password attempts
Limiting our password checking routine to only three password guesses is a bit more challenging, because the number of guesses has to persist across CPU reboots. `EEPROM.write` and `EEPROM.read` are just ways that Arduino manages persistent memory. First argument for both method is a memory address - the place where we will be storing our counter.

```
bool checkPass(String buffer) {
  byte tries = EEPROM.read(TRIES_ADDR);
  if (tries == 0) {
    return false;
  }
  bool result = true;
  for (int i = 0; i < PASSWORD.length(); i++) {
    if (buffer[i] != PASSWORD[i]) {
      result = false;
    }
  }
  EEPROM.write(TRIES_ADDR, tries - 1);
  return result;
}
```

The method first checks if there are any tries left. If there aren't it returns `false`. If there are some tries left it will perform regular password validation and then decrement the counter. Now we can rest easy that our password check routine is secure.

Well, not really. The thing is that in the main code we have this:

```
  bool correct = checkPass(pass);
  if (correct) {
    Serial.println("Password correct!");
```

This means that there is a signle point of failure - if the execution of checkPass fails (for whatever reason) and erroneously produces `true` the board will be unlocked with a `Password correct` message.

> This is a point in the training where most of the participants have either a very confused look or think I'm joking. It's fine if you had the same reaction, as the rest of this course deals with what can be described as magic.

If we can somehow inject a fault and make the `checkPass` method believe that the password is correct we can unlock the board. We do not even have to know the password in order to do that.
