### Many highs and lows this week.
###### (If you want to see cool pictures, scroll to the end!)

Good news:  I have all the Geospacial tools working, and a U-net I can train.
Bad news:  The [MDA Flood Analysis Shapefile](https://www.digitalglobe.com/opendata/hurricane-harvey/vector-data) I was going to use for my labels turned out not to be very reliable.  There are lots of little false positives, it is very low resolution compaired to the images, and huge amounts of flooding are missed.  Training the U-net on it didn't work well at all.

**Quality Data Labels are everything in machine learning** (though the same is often said about feature engineering).  This was a major setback.  Do I need to label the images by hand myself?  Do I need to find some some swampland or muddy rivers to train on using more detailed OpenStreetMaps data?  

NO!  While the color of floodwater varies quite a bit from place to place, it is almost always close to uniform in color at any location.  This means that it is a prime candiate for 'old fashioned' image segmentation!  I looked around for a good image analysis library to use, but they weren't really optomized to my task:  to extract a single segmented surface without manual input over a large number of images.  So... I made my own.

Or to be more specific I employed a unsupervise clustering agorithm called DBSCAN to find all the major clusters of colors (also used spacial data, but had to down-weigh that feature compaired to color to get the clusters to cross over barriers).  I then wrote some checks to reject clusters that looked too much like foliage or pavement, after which the most common surface is flood water!  At least in flooded areas... so I will be using the course MDA shapefile data to identify images with flooding, then run my DBSCAN image segmentation on them to make accurage pixel-level masks of those flooded regions.  These, and images of some non-flooded areas should be a MUCH better training set.

I'm sure it will need a little more tuning, but I think it's extremely cool how well this works for such a simple method.

### Example of unsupervised training mask 'improvement':
<img src="../images/Screen Shot 2017-09-08 at 4.42.07 PM.png">
<img src="../images/Screen Shot 2017-09-08 at 4.42.31 PM.png">
<img src="../images/Screen Shot 2017-09-08 at 4.42.44 PM.png">
