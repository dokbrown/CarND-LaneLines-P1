## Writeup for Project 1 - CarND-LaneLines-P1
Austin Brown

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on my work in a written report (this document)


[//]: # (Image References)

[image1]: ./test_images/solideWhiteCurve.jpg "input"
[image2]: ./test_images_output/solideWhiteCurve.jpg "output"

---

### Reflection

#### 1. Pipeline Description

My pipeline consisted of 8 steps.
1) Converted the image to Grayscale
2) Applied gaussian smoothing
3) Applied Canny edge detection
4) applied a polygon region
5) applied hough lines to identify all line segments
6) split the image into two halves to separately identify each lane line
7) average all line segments in each half to create a representative line
8) create the lines and add on top of the image

In order to draw a single line on the left and right lanes, I modified my pipeline by considering the left and right halves separately and using the output of the hough transform as a set of line segments. I averaged the segments on each side weighted based on their length.

This is an example before and after image:

![input image][image1]
![output image][image2]



#### 2. Potential Shortcoming

On some video frames, the averaging produced slopes that were 0, inf, or NaN. In this iteration, the pipeline ignores those lines.

Running the pipeline on the challenge video demonstrated additional flaws. One initially was that the resolution was different, and I had hardcoded the length and width of the frames. I modified to measure the size and use fractions to define the masks so it will scale with the image

Another challenge not yet addressed is that the challenge feed does not extend to the end of the image. This leads to strong edge detection on the hood, giving nonsense lines as output.

A third likely challenge is that the challenge video has a stronger curve, which will make it impossible to represent accurately with any single line segment.

Lastly, a optimized my parameters based on the still images but there are frames that do not have good fit in each of the videos.


#### 3. Possible Improvements

A possible improvement would be to crop out the hood portion in the mask to avoid detecting those edges

Another potential improvement could be to change the line representation fundamentally to a polynomial curve. This would require more extensive fitting than the hough lines allows.

For the frames with poor fit, a potential improvement would be to output the intermediate steps for those frames only (by adding diagnostic code to output images at each stage for a coded set of problem frames). I could then tune parameteres to improve fit for those problem frames.
