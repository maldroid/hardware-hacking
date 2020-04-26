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
\- | `i < 5`
\- | `read PASSWORD[1]`
\- | `read buffer[1]`
\- | `compare two values`
\- | `return false`

As you can see in the first case the `checkPass` `for` loop does only one iteration (since the first character is incorrect) and in the second case it does two (since the first character is correct, but the second one is incorrect). Can we detect this timing difference? Let's take a look!

> Download this Logic data dump. Load it in Logic by clicking on "Options" (upper left corner) and then "Open capture / setup". Choose the file you downloaded.

You should see a window similar to the one in the screenshot below.

## Figuring out the protocol

As you know form the code the protocol we are using is a serial (or UART) protocol. This means that the data is sent and received using two different wires (or channels). One - TX - is used to transmit the data from the board to our computer and the other one - RX - is used to end the data from the computer to the board where our password checking code runs. You can see how the connections work in the picture below.
