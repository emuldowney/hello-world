---
layout:     post
title:      I thought I could be a mechanical engineer
date:       2020-04-13
summary:    I tried to shape some semi-rigid coax
categories: electronics
tags: electronics
#image: "images/15.jpg"
---

## I needed to build something permanent
So for work, I needed to make the connection between a couple of RF components. I had a 4-way splitter and a 4-port RF switch and wanted to connect the two together for some testing. Each cable would carry the exact same signal, split from the splitter, to the switch, which would then select a port for passing the signal on towards the Device Under Test. The catch was that each cable should be a slightly different length, to phase shift the signal. Let's just say that the amount of phase shift didn't matter, it was covering a frequency band of 6-8GHz anyway, so the shift would depend on the frequency. That means that the actual cable lengths didn't matter.

<div class="showcase" align="center">
  <img style="width:50%" src="https://www.minicircuits.com/images/case_style/Z184.png" alt="A 4-port RF splitter from Mini-Circuits">
    <p class="meta">The RF splitter</p>
  <img style="width:50%" src="https://www.minicircuits.com/images/model/RC-1SP4T-26.png" alt="A 4-port RF switch from Mini-Circuits">
    <p class="meta">The 4-port RF switch</p>
</div>

## Design plan

I thought it should be easy enough to connect the two devices together. My original concept was to do something like this image here.
<div align="center">
  <img style="width:50%" src="/images/rigid-coax-post/initial-concept-drawing.png" alt="Concept drawing">
</div>

Then to connect the ports, I could do something similar to this.
<div align="center">
  <img style="width:50%" src="/images/rigid-coax-post/theoretical-measurements.png" alt="Drawing showing 45degrees angle">
</div>

Taking the datasheet for the [RF switch](https://www.minicircuits.com/pdfs/RC-1SP4T-26.pdf), and for the [RF splitter](https://www.minicircuits.com/pdfs/ZC4PD-153+.pdf), we can get the actual sizes for these devices. The SMA spacing is the most important detail here, 12.7mm (P) for the splitter, and 13.5mm (F) / 23.4mm (J) for the switch. So if we take the image above, and look at length Y, and conceive this length as the spacing between each of the sma connectors. In order to achieve a single bend between the splitter and the switch, we can do a 45 degrees bend between two parallel connections. The length of the bend should be ```y*sqrt(2)``` (or ```y*1.414```). This will account for the different length cables.
However, in order to take into account the position of sma connectors, I need to count more than one sma spacing.
I will replace y with the respective letters from the datasheets below.

| RF switch port | RF splitter port   | cable length                              | actual cable length change |
| ---------------| ------------------ | --------------                            | -------------------------- |
|      1         |         4          |   No change                               | 0 mm |
|      2         |         1          | ```P*sqrt(2) + J*sqrt(2)```               | ~5 mm |
|      3         |         2          | ```2*P*sqrt(2) - F*sqrt(2)```             | ~15 mm |
|      4         |         3          | ```3*P*sqrt(2) - F*sqrt(2) + J*sqrt(2)``` | ~21 mm |

## Manufacturing

So, when I made those cables (This youtube [video](https://www.youtube.com/watch?v=Mp96xNic7ho) is a great tutorial for adding SMA connectors to those cables.), they measured up quite well to the table above. I tried to connect the cables. I quickly realised that the one bend idea was not going to work. The 5 mm cable just wasn't going to fit in its position. It wouldn't even fit in any position combination.

However, it would be remiss of me, if I didn't make them fit. There were a few extra bends required to make them fit, but it all worked out well in the end.

<div align="center">
  <img style="width:50%" src="/images/rigid-coax-post/rigid-coax-finished.jpg" alt="Finished Product. It might have a few extra bends.">
</div>



