---
layout:     post
title:      Mechanical Keyboard world
date:       2020-10-01
summary:    Oh no I didn't!
categories: electronics
tags: electronics
image: "images/16.jpg"
---

## First mechanical keyboard

There is so much to the world of mechanical keyboards. Of course the best place to start is on the subreddit [/r/mechanicalkeyboards](https://www.reddit.com/r/MechanicalKeyboards/). I got a little sucked into that world, about 6 months ago, when I was having wrist issues, and suspected I was developing RSI (repetitive strain injury) from using laptop keyboards. I went in search of a split keyboard design that would allow my wrists to settle at shoulder width apart. It seems [Ergodox EZ](https://ergodox-ez.com/) is the best out there, or maybe they just have the best marketing.

I didn't intend to spend €300 on a keyboard that I didn't know much about. I also couldn't justify spending that much money on a keyboard to anyone around me. So what does a good engineer do? He buys all the parts and builds his own. In truth, I didn't fully think the ergodox was for me. There were a lot of complaints that the thumb cluster didn't suit small hands. I searched for a keyboard designed better to my needs - split design, home manufacturable, cool-factor.

## Enter Sofle
[Sofle](https://josef-adamcik.cz/electronics/let-me-introduce-you-sofle-keyboard-split-keyboard-based-on-lily58.html) just seemed like a nice keyboard. Josef had a great story behind it, and had put lots of thought into the changes that he made to the lily58. I liked the option to add potentiometers to the board, and his build log was very thorough. It was as simple as download his KiCad files and send them off for manufacturing.

This is where the story takes a pause... After all, it is March or April, while the whole world is shutting down due to Covid-19, and even though China was opening up again, the world postal services were just overwhelmed with packages and deliveries.

While we wait let us take a deeper look at this keyboard. I forked Josef's [github](https://github.com/josefadamcik/SofleKeyboard), and made a few aestethic changes to the keyboard. Mostly, just adding my name and website to the backplate. (I will get the repo up on github in time. I needed to verify it was working before letting it out on the world.) His design is a 6x4+5 keyboard, with matching plates for each hand. The PCBs can be flipped over and keys fitted on the back-side to make the second hand. I generated the gerbers, and loaded them up on [JLCPCB](http://jlcpcb.com), to order. A grand total of €38.69 shipped. The next step was to pick out all the remaining components on AliExpress, and wait.

### Key Switches
I'm not someone who loves the sound of mechanical clicking switches. Also with all the remote working now, I don't need to have all my phone calls punctuated by the clickity-clack of my keyboard. For this reason, I had to go with Linear switches, as the quietest. I didn't splash out for silent switches, and I don't think they are any louder than a laptop keyboard so they are sufficient. I looked at lots of forum posts about the pros and cons of all the different types of switches, and then looked at the technical specification sheets for the switches. The detail is amazing for such a _'simple'_ component. In the end I bit the bullet and chose the Kalih silver switches, with 1.1mm of travel and 40g of force. As you will see in the conclusions later I think I could have gone for ones that required a little more force, because I ended up tapping some keys accidentally.

### Key Caps
Ahhh! What a rabbit hole! Who pays $50 for a single keycap?! I guess that was handcrafted and sculpted, but it isn't the kind of purchase I would make. I found it quite difficult to get any nice keycaps in stock anywhere. ISO key markings were vital for the UK layout, and I tried to find DSA caps, but there were none available. In the end I popped for a nice beige and grey set, albeit shaped for a standard keyboard. I will look out for a nice DSA set in the future. (One might notice the 0-key in the photo above is upside down :)

## Finally, all the components

All throughout the summer I would get random surprise packages in the post, a bag of diodes here, an audio cable there, some OLED screens... You get the idea. Eventually, last week, I had all the necessary components to get started on the build. As my luck would turn out, the Arduino Pro Micros didn't fit the PCB! After a little creative bending of headers, and some extra forcing of components to fit around them, I squeezed them onto the board. I didn't populate the OLEDs, because they looked delicate, and might be more difficult to salvage, should the PCB not work.
The quick test with a tweezers went fine on the first PCB, so I started populating the key switches. The second PCB just would not pass the tweezers test. In fact, the power LED on the Pro Micro was not lighting up. This gave the biggest hint to the problem. The TRS cable carries power, ground and serial over an audio cable from the 'controller' left half of the keyboard to the second half of the keyboard. A quick test with a multimeter, and sure enough, no power. I did a quick continuity test on the cable alone, and there was no contact at all (This is what you get for 50c on AliExpress). I borrowed a cable from a friend at the office, and sure enough, the Pro Micro powered up. Problem solved. I threw out AliExpress's finest, but cut it open first to check if there were wires in there at all - there were. I was not worried, I would find a replacement elsewhere.
Fitting the key switches on the second PCB were more difficult. The Pro Micro that I had to wiggle and worm to fit to the board were blocking two of the switches. With a little bit of force, everything could be made fit. Time to test it out as a pair of boards. My initial tests showed it seemed very slow. There were lots of key press misses, and the right hand side basically didn't send any key presses at all, unless you were lucky. I figured there had to be something simple wrong. Well Josef had excellent documentation on his build, and sure enough down in the troubleshooting section of his [build log](https://josef-adamcik.cz/electronics/soflekeyboard-build-log-and-build-guide.html) there was the root cause, the  lack of OLED. Five minutes of soldering, and I was back up and running. Now the keyboard was much more responsive.

## Conclusion
So I have been using the keyboard for 3 days now in work. It is like learning to type all over again. The Kalih silvers are very sensitive, and I like to rest my fingers on the keys between typing, so I have a lot of phantom keypresses. The switch to ortholinear is also causing lots of double keypresses. However, the biggest problem I have found so far is that I like to press the letter 'B' with my right-hand. As you will note from the photo above, the 'B' key is on the left half of the keyboard. This has taken a lot of getting used to. There are a lot of 'N's in place of 'B's in much of my work this week.

The extra force required to fit all the key switches beside the extra-large Pro Micro board is causing a small bend in the PCB. This bend is causing the keyboard to rock on the desk, which is quite frustrating. I have gel pads on the back, but they are not fully resolving the problem. The PCB spacers have not yet arrived. Once they arrive, I can add the backplate, which will ensure a level stand to rest on the table. Whether the keyboard is too high for my hands then remains to be seen.

I think I will re-program some of the key locations when I get to know exactly what I need. When programming in python and C, there are certain syntactical characters that are very useful, and making them easier to access will make my life a lot easier. The keys that I struggle with most right now are the underscore, delete, and backslash. I also prefer to have the ESC as the top left key. 

I like the keyboard construction, I think it is very usable. I look forward to being able to add the pots to the PCBs, if they would only arrive in the post!

## Bill of Materials
* Keycaps: [https://www.aliexpress.com/item/32836997999.html](https://www.aliexpress.com/item/32836997999.html)
* Headphone Jack: [https://www.aliexpress.com/item/32869968774.html](https://www.aliexpress.com/item/32869968774.html)
* TRS cable (the good one): [https://www.aliexpress.com/item/32889700779.html](https://www.aliexpress.com/item/32889700779.html)
* 1N4148 Diodes: [https://www.aliexpress.com/item/32251916196.html](https://www.aliexpress.com/item/32251916196.html)
* Kalih Switches: [https://www.aliexpress.com/item/32836139359.html](https://www.aliexpress.com/item/32836139359.html)
* OLED screen: [https://www.aliexpress.com/item/32712441521.html](https://www.aliexpress.com/item/32712441521.html)
* Switch sockets: [https://www.aliexpress.com/item/32959301642.html](https://www.aliexpress.com/item/32959301642.html)
* Pot Knob: [https://www.aliexpress.com/item/32753511277.html](https://www.aliexpress.com/item/32753511277.html)
* Rotary encoder: [https://www.aliexpress.com/item/32382989585.html](https://www.aliexpress.com/item/32382989585.html)
* Gel pads: [https://www.aliexpress.com/item/32839661456.html](https://www.aliexpress.com/item/32839661456.html)
* Pro Micro (WARNING: I ordered the mini-B USB which is too wide, the micro-B is the correct size): [https://www.aliexpress.com/item/32849563958.html](https://www.aliexpress.com/item/32849563958.html)

