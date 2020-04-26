# Timing attack

As you might've guessed from the title of this page the answer is: **timing attack**. There's a difference in time it takes to validate the password based on the number of characters that are correct. Take a look at step-by-step `checkPass` verification of two password tries below -- one with all characters incorrect and one with one correct character.

`qqqqq` | `pqqqq`
------------ | -------------
`i = 0` | `i = 0`
`i < 5` | `i < 5`
`read PASSWORD[0]` | `read PASSWORD[0]`
`read buffer[0]` | `read buffer[0]`
`compare two values` | `compare two values`
`return false` | `i = 1`
\- | `read PASSWORD[1]`
\- | `read buffer[1]`
\- | `compare two values`
\- | `return false`

As you can see in the first case the `checkPass` `for` loop does only one iteration (since the first character is incorrect) and in the second case it does two (since the first character is correct, but the second one is incorrect).
