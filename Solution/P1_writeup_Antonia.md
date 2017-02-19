#**Finding Lane Lines on the Road** 


**Antonia Reiter, 19.02.2017**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[image1]: solidWhiteCurve_Annotated.jpg "solidWhiteCurve_Annotated"
[image2]: solidWhiteRight_Annotated.jpg "solidWhiteRight_Annotated"
[image3]: solidYellow_Left_Annotated.jpg "solidYellow_Left_Annotated"
[image4]: solidYellowCurve_Annotated.jpg "solidYellowCurve_Annotated"
[image5]: solidYellowCurve_Annotated2.jpg "solidYellowCurve_Annotated2"
[image6]: whiteCarLaneSwitch_Annotated.jpg "whiteCarLaneSwitch_Annotated"

[video1]: white.mp4 "White_Annotated"
[video2]: yellow.mp4 "Yellow_Annotated"

---

### Reflection

See my solution notebook at [P1_Antonia.ipynb](https://github.com/AntoniaSophia/CarND-LaneLines-P1/blob/master/Solution/P1_Antonia.ipynb) 


###1. Description of my pipeline

My pipeline consists of the following steps:
####A Define the grayscaled picture including the gaussian blur
####B Apply the Canny algorithm with ration 1:3 between lower threshold and higher threshold
####C Definition of the vertices for the region of interest
####D Selection of the region of interest
####E Hough transformation
####F Overlay of the original picture with the annotations

The draw_lines() function is done like that:
- I know that I have a set of small lines which can be sorted into equivalence classes 
- I can differentiate the different equivalence classes be using the slope and the intersection
- From the example pictures I already know that I will only have two equivalence classes and nearly no "noise"
- Thats why the simplest way will work out: separating in the two classes with positive and negative slope
- After separating the lines into the two equivalence classes a simple polynomial fit over all equivalance classes gives me slope and intersect of the interpolated line
- Now drawing this line as a thick line is easy and it automatically is centered as the lines contain both the left and the right boundaries of a road lane
- Additionally I calculated the maximum and minimun x-value for both lines in order the really reflect the identified lines. But of course a extrapolation to the bottom of the picture would be somehow trivial


#####Here are some annotated results:
![solidWhiteCurve_Annotated.jpg][image1]
![solidWhiteRight_Annotated.jpg][image2]
![solidYellow_Left_Annotated.jpg][image3]
![solidYellowCurve_Annotated.jpg][image4]
![solidYellowCurve_Annotated2.jpg][image5]
![whiteCarLaneSwitch_Annotated.jpg][image6]

#####And here are my videos: 
![White_Annotated][video1]
![Yellow Annotated][video2]


###2. Identify potential shortcomings with your current pipeline

Actually I'm not really happy at all about hardcoding the vertices - but it seems to work fine with pictures of this size.
Although the orientation and angle of the camera doesn't change over time the main prerequisite is that also the horizon level doesn't move.
This prerequisite is true for flat streets, but is doesn't hold for mountainous roads!

Another shortcoming is the algorithm of finding equivalence classes. I chose the most simple approach as it was sufficient.
I haven't tried out the "challenge" until now but I assume there are complex situations contained like more than two road lanes in the picture or "noise".
With "noise" I mean smaller lines which do not fit to any road lane (e.g. white symbols like arrows on the road)
Also this algorithm was only applied for examples with perfect weather conditions - I guess factors like wind, rain or even snow (white! **arrrg**) will disturb this algorithm heavily.

Furthermore I was really struggling whether it is better to do selection of the region of interest before or after the Hough transformation - but seems to work perfectly fine for these examples....
- possible advantage of doing region selection before: less effort for Hough transformation
- possible disadvantage of doing region selection before: in case the re-transformation would return lines outside the region of interest I have to apply region selection again

Unfortunately I had no time to investigate this further but I'm sure that the latter can happen easily.



###3. Suggest possible improvements to your pipeline

- At first I would check whether the selection of region should be done before or after the Hough transformation - possibly the most efficient and robust way is to do it before and after the Hough transformation.
- Secondly the draw_lines function must be improved in order to deal with more than two equivalence classes. Actually a more general function of splitting up line any number of equivalence classes should be implemented. I'm nearly sure that Python has already some functions like, but unfortunately I'm a beginner in Python so I definitely had to investigate further
- Thirdly I would try to figure out also "noise" in that list of lines, e.g. if we get 5 equivalence classes with let's say 20,18,25,3 and 30 elements this could be an indicator that the equivalence class with 3 elements could be noise (of course also other characteristics like overall length or number of covered pixels of an equivalence class could be a criteria)
- At last I would try to figure out what could be done in order to get a better calculation for the region of interest. Possibly using inertial sensors (which are contained in every mobile phone nowadays) could give a hint for orientation of the horizon line.

Finally I'm really curious to see what improvements can be done (a later lesson is called "advance lane detection") and what benefits eventually deep learning could give!

To conclude with: I had a lot of fun working on that project and enjoyed very much!! Hope it will continue exactly like that!! :-)