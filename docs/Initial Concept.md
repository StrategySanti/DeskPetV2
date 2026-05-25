My idea: A small gadget that sits on your desk that has a screen that displays useful information like the time and temperature.

I wanted to try and build something like this because I often put my phone away while working to avoid getting distracted. This means that I cant quickly check the time at a glance and cant use my phone for timers and alarms as easily. Plus, I can make the gadget have a cool theme to add a little colour and character to my computer setup.

This project would involve a microcontroller, a screen and some sort of inputs to give the user controls over the different functions. This actually started a copy of a previous project I created which ended up failing as a result of poor planning and goals on my part. The function of the original version was to simply display images of cartoon character with buttons to allow you to switch between different characters and variations of their faces. I quickly realised that this provided almost no benefit whatsoever and I wasnt actually a fan of how large it became.

![Front of V1](docs/Images/IMG_4612.png)

For the inputs, my requirements are:

Inputs should feel simple
Use must feel seamless or satisfying
Simple logic
Small in size

Using a rotary encoder would be the best because it gives the user a feeling of feedback when using it. The user experience becomes satisfying and makes interactions feel real. Another big benefit of using rotary encoders is they can be rotated infinitely in either direction, making the input versatile and easily expandable. From past experience when working with rotary encoders, they are difficult to use correctly and I often fail, meaning that I need to learn how to use them properly if I choose to use one.

Capacitive touch sensors feel completely opposite to a rotary encoder, since you don't even need to touch them for a detection. These are ideal because they are easy to hide, which opens up the possibility for custom buttons that fit the theme much better. Their logic is a very simple ON/OFF signal directly into a microcontroller pin, which makes them very easy to use. A drawback is that if the screen lags and the user doesn't see any change in the image, then they might repeat the same input, which the microcontroller can still read and make updates with. This means that my code would need to be robust and optimised.

Standard pushbuttons also work  well, but they are harder to hide and aren't ideal for making a simple button mechanism. They require the use of pullup resistors, which most microcontrollers have built in, so using them is also simple. To use these, I would need to create a PCB to hold them, otherwise they become a mess of jumper wires and super glue. I plan to create a PCB in the future depending on what microcontroller I use.

The best choice for the microcontroller would be an ESP32 since they are cheap, have WiFi and Bluetooth built in, and have much more power than an Arduino. Since I plan on keeping the pet plugged in at all times anyway, I can just use a generic ESP32-WROOM-32 dev board from AliExpress with built in power regulation and USB serial. This means that if I create a PCB, then the board would have to be soldered on with the Single-In-Line connectors on it.
