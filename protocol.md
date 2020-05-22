# How does a serial protocol work?
As you already know serial protocol uses two channels to communicate - RX and TX. The data on both of these channels is encoded in the same way. As you've also noticed when the channel is idle (no data is being transmitted) the channel is set to high.

Serial protocol has several parameters which `picocom` presented to us when we've run the serial console:

```
$ picocom /dev/ttyACM0 -b 115200
picocom v3.1

port is        : /dev/ttyACM0
flowcontrol    : none
baudrate is    : 115200
parity is      : none
databits are   : 8
stopbits are   : 1
```

The serial "frame" (one unit of communication) looks like this:

Start bit | Data bits | Parity bit | Stop bit
------- | ------ | ------ | ------
Always one bit  | A number of data bits | One bit for error correction | A number of stop bits
Always low | high if "1", low if "0" | Depends on the algorithm | Always high

The parameters that control the number of bits are called *D* (for "data bits"), *P* (for "parity bit") and *S* (for "stop bits"). The default values for these are:

* D = 8 (bits)
* P = N (none - there is no parity bit)
* S = 1 (there's one parity bit)

Although different serial protocol implementations may use different values. ATMega CPU is using the default values, which means that each frame will start with one start bit (low), then we will have 8 bits of data (low/high) and one stop bit (high). Let's take a look at our Logic software capture and try to decipher the first byte (= 8 bits = 1 frame).

Fortunately Logic software contains a serial protocol analyser. In order to enable it click on the `+` sign next to the `Analysers` box on the right hand side and choose `Async serial`. Choose Channel 1 and click on "Use Autobaud". Leave all the others parameters as is.
