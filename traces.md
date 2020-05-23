# Looking at the power traces
[Download this Logic capture](assets/power_analysis.logicdata.7z) and open it in the Logic software. You should see one second capture with 5 password tries. The password have more and more correct characters up to the actual password. Under the TX and RX channels you can see the analg channel that captures voltage accross the resistor, which corresponds to the power draw of the CPU. If you zoom in on the period between the first password guess and the first response you should see something similar to the screenshot below (notice how the blue boxes on the top are positioned).

![Power trace of the first password try](assets/logic-screenshot-power-loops.png)

Now the question becomes: where is `checkPass`? We're looking for five loop iterations, all of which will have the same instructions (since all the characters are wrong) and the same power trace. There's a couple of plausible places, but there's a place between two "squigly" patterns which seems to have the same power trace five times. Take a look at the screenshots below.

![Annotated power trace](assets/logic-screenshot-power-loops-annotated.png)
![Annotated loop iterations power trace](assets/logic-screenshot-power-loops-iterations-annotated.png)

Hopefully you can see all the 5 loop iterations (i.e. 5 same power traces). Now that you guessed where the for loop is (with a little bit of help).
