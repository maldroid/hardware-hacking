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

Well, there's still a bi of problem. Now depending on how many password characters are false the check will take longer (the `result` variable has to be updated).
