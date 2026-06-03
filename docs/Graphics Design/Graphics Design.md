One of the main goals for the desk pet is to stick to the theme of Fallout by creating a Fallout inspired user interface. I want the design of the UI to match the look of the Pip-Boy from fallout 4. In Fallout, the Pip-Boy is a wrist mounted device which acts as the player's inventory, map, stats screen, radio etc. It features a large screen and various knobs and buttons which are used to switch between screens within the menu. The reason I want to match this look is because I thought the design of the menu was very intuitive and easy to understand since most options are displayed constantly, meaning the user never has to spend ages searching for the specific thing they want. I want to adopt this design partly for its functionality, but also because its easily Fallout's most recognisable gadget.



(Image of Pip-Boy)



I am using an ESP32 with a 1.5" OLED screen with a CH1117 driver. I chose to use the u8g2 library because it is the library I am most familiar with and I found it to be the most versatile library out of all the options. To design the graphics for my 1.5" OLED screen, there are many methods, however the one I chose in the previous version of this project was this:



* Design the graphics using GIMP on a 128x64 pixel canvas
* Export the graphic as a PNG
* Import the graphic into ImageToCPP.com
* Select settings which match the screen, driver and graphics library
* Export the Bitmap code and bring it into Arduino IDE
* Use u8g2.drawXBMP() to display the bitmap



![Screenshot of GIMP](Images/GIMP.png)

(Screenshot of imagetocpp.com)

(Screenshot of bitmap code)

(Image of bitmap on OLED screen)



from my understanding, Bitmaps are images which have been formatted as raw pixel data, where each pixel has a colour value. A graphics library like u8g2 then takes that raw pixel data and sends it to the screen's driver through I2C. I would have preferred to use the SPI interface because it is much faster, which would result in cleaner screen updates and better animations, however the screen I have is I2C. The only other SPI screen I have is a 2.5" TFT screen which I didn't want to use since because I don't need the full RGB spectrum for the graphics. In Fallout, the Pip-Boy screen only displays the colour green, so having full RGB only bulks up the bitmap data and reduces the number of bitmaps I can store on my ESP32. For the method described, all bitmaps are stored in PROGMEM, which is the storage for the programme that the ESP32 runs. This method is the only one that works well because otherwise, Arduino IDE stores the bitmaps as active variables within memory which greatly reduces the number I can store, as well as taking up memory which could be used by the code instead. This is an issue I faced in the previous version where I could only store 3 full screen bitmaps before the ESP32 ran out of memory to store more. This was a problem because I wanted to be able to display as many bitmaps as I wanted with the possibility to add more in the future.



I felt that this was the most robust method at the time since it meant I had complete control over what the design looked like. However, something I wasn't particularly fond of was how massive the Bitmap data was, making my code many hundreds of lines long despite my code only being around 200 lines long. I also felt that setting up the bit map within the code involved a relatively complex process which I struggled to find good information for online. This involved importing the bitmaps into a separate file within the same code environment as a custom library which held all of the data. From here, all I had to do was call the variable with the name of the bitmap and draw it onto the screen. This method is ideal for complex images like characters or animations since there isn't another good way to draw them using the U8G2 library. This method isn't ideal for displaying simple shapes, straight lines and text, since the U8G2 library has built-in functions that simplify this process, such as:



u8g2.drawRect()

u8g2.drawLine()

u8g2.print()



Another method I found online was a website called Lopaka.app. This website is created specifically with the goal of creating graphics for small screens used in Arduino projects. This website has tools for basic shapes and pixels, as well as a built-in image to bitmap converter like the one found on imagetocpp.com. This website also generates the code for the specified library which can easily be pasted into Arduino IDE. I found this to be very useful for creating some drafts for the menu since you can easily move and manipulate the shapes and see exactly how they would end up on the OLED screen. A limitation I found for this website is the free version having limited screens you can work on, meaning its harder to create multiple different menu screens without paying for the full service. However, this isn't an issue since I found the free version to work just fine for my use case and I do plan on paying for the service once if I continue to use it for creating graphics for other projects.



(screenshot of Lopaka.app)

(screenshot of fist menu design)



I am choosing to use a combination of the 2 methods because each one has its strengths and I feel they both complement eachother where the other lacks, making a near seamless process for creating the UI for the Desk Pet. I decided to avoid using Lopaka.app for the entire process because I am using the free version and I still think the method of using GIMP to be much better since it is much easier to import images there. I will use Lopaka.app for simple graphics like displaying text and the main features of the menu. With this introduction to the methods complete, its time to actually start creating the graphics.



After creating the first draft for the top menu in Lopaka.app, I added the code to my project and tested the look on the screen. I was quite happy with how it turned out, especially given how simple the design is. One very small issue I noticed was that when the menu is changed to the "Tools" section, the entire top bar moved up a pixel. After some brief searching, I found out that the X and Y values for everything in the "Tools" section was a single pixel higher. This was an easy fix, where I just changed the Y values for that part and it turned out perfect. After coding the pushbuttons to change between the different menus, I though the menu looked really good and was happy to continue.



The next part I wanted to work on was creating a custom font for the numbers. I decided to do this because I didn't like the look of any of the fonts that are part of the u8g2 library because neither of them were the size I was looking for and I also wanted a custom look for them. I started designing the numbers in GIMP as 15x20 pixel images and after completing the first draft for numbers 0-9, I used image2cpp.com to convert the PNGs of my numbers into bitmaps. I had to do some testing to make sure I selected the right options for the u8g2 library. For example, I designed the numbers using black on a white background, but my screen is inverted, with white as the actual pixel, so I had to make sure to select the "Invert Colours" option. 



(screenshot of numbers in GIMP)



An issue I ran into with the selection of options was the bitmaps appearing distorted. This is something that the website warns about being a possibility and the fix I found for it was to change the "Draw Mode" option to "Horizontal - 1 bit per pixel" and to select the "Swap bits in byte" option which the website states helps when working with the u8g2 library. My first test to display the number 0 came out perfectly after figuring out the correct options for the settings.



(image of distorted bitmap)

(video of working custom numbers and time)



I then made some small changes to the positioning of the numbers so they sit in the centre of the screen. With this change made, the "Time" page is complete.



(image of numbers iter 1 in gimp)



While working on designing the "Tools" page on Lopaka.app, I noticed that the website allows you to upload an image and convert it into a bitmap. This sparked my curiosity, so I had a look at some of the pre-made images available on the website and really liked the look of a gear that was available. After taking a break from working on the different pages, I thought that displaying the temperature separately from the time makes it more inconvenient to see. This would be failing at the main goal I wanted to solve with this project: convenience. A combination of these 2 discoveries led me to start designing a better interface for the gadget. I will again take inspiration from fallout and try to design something similar.

