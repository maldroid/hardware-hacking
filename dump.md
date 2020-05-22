> Download this Logic capture dump. Load it in Logic by clicking on "Options" (upper left corner) and then "Open capture / setup". Choose the file you downloaded.

You should see a window similar to the one in the screenshot below.

## Figuring out the protocol

As you know form the code the protocol we are using is a serial (or UART) protocol. This means that the data is sent and received using two different wires (or channels). One - TX - is used to transmit the data from the board to our computer and the other one - RX - is used to end the data from the computer to the board where our password checking code runs. You can see how the connections work in the picture below.
