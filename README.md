# Lane-Detection-System-using-Hough-Transform
## Overview
When we drive, we use our eyes to decide where to go. The lines on the road that show us where
the lanes are act as our constant reference for where to steer the vehicle. Naturally, one of the first
things that is required for developing a self-driving car is to automatically detect lane lines using
an algorithm. In this assignment we are going to learn about the implementation of a very important
component of self-driving car called lane detection.

## Implementation
There are some basics steps for the implementation of lane detection, that are discussed below.
But you are not restricted to follow the same steps, we encourage you to put your own thoughts
and come up with a better algorithm to solve this problem. The algorithm steps have been adopted
from the Udacity Self-driving car – Project 1.
 
1. Convert the input RGB image (I) to the HSV space. You can use cv2 for it function. Create
a binary image B of same size (row and column) of RGB image. Set all the pixels in the B
to 1.

HSV, it is basically a different way to describe color than the classic RGB we are used to,
you can learn more about it on Wikipedia. We will explain later why this conversion is
necessary. Also have a look at this table
http://www.rapidtables.com/convert/color/index.htm

![image](https://user-images.githubusercontent.com/105145104/168329870-112e03ae-026d-47c2-a46c-d5ad1d639ce3.png)


2. Remove the noise by applying Gaussian filter. This should also remove small artifacts and
small particles.

3. Filter out (turn black, set to zero) any pixel (in the binary image B) that is not the color of
the lanes (in HSV space), in our case white or yellow. Because lines on road are marked
with yellow or white color. You should get a result as in Fig. 2.
First you need to check which values of H, S and V produce a white or yellow color. Use
these values of H, S and V as threshold and filter out corresponding pixel locations. Now
turn black (set to zero) any pixel that is not in the filtered locations. This way you will be
able to filter white or yellow pixel locations.

![image](https://user-images.githubusercontent.com/105145104/168330083-5a15f344-cbb8-47ae-a22a-45085e89cf67.png)

You will notice that HSV has allowed us to remove unwanted pixels with much more ease. We
have to look to both Hue and Saturation of the pixel (which color it is and how intense the color is),
giving us a room of adjustment for “value” (how dark it is). This setup allows us to handle
shadows and lighting variations. To extract the yellow and white color you will need to study about
HSV color space. (http://www.rapidtables.com/convert/color/index.htm)
There only remains the pixels with white or yellowish tint, with some false positives but our two
lines are very clearly defined.

![image](https://user-images.githubusercontent.com/105145104/168330231-c519b023-3f00-4010-b342-0231c6e94b8c.png)

4. Convert the input image I to the grey scale image Ig. Use the Canny edge detector to
detect the edges. The Canny algorithm detects edges on a picture by looking for quick
changes in color between a pixel and its neighbours, in this case between the white or
yellow lane line and black pixels. Use the binary image B to remove all the edge pixels
(set to zero) which are set to zero in I. You can use Python’s cv2 Canny Edge Detector or
the one you designed yourself in Assignment 2.

![image](https://user-images.githubusercontent.com/105145104/168330440-e5284f73-9c8f-493f-ae29-2473dc1fef0c.png)

5. Now we define region of interest. Given the position and orientation of the camera, you
know that the lanes will be in the lower half of the image, usually in a trapezoid covering
the bottom corners and the centre. You don’t want your region to be too narrow and have
the lines out of our region of interest.

![image](https://user-images.githubusercontent.com/105145104/168330626-d774860e-ea00-40e9-aa57-878ece22c29f.png)

6. Run Hough transform to detect lines on our image. What does Hough transform do? To
summarize quickly, it allows us to easily extract all the lines passing through each of our
edge points and group by similarity, thus grouping together the points that are roughly
located on a same line. After the above step we will still get multiple lines, but we only want 2 distinct lines (to
form the lane) for our car to drive in between.

7. Filter the lines that only have horizontal slopes. Based on value of slopes distinguish and
group the lines into left lines and right lines.

8. Last step is to compute the linear regression of both groups and draw the results on your
region of interest. Simply, linear regression is an attempt at finding the relationship
between a group of points, so basically finding the line that passes at the closest possible
distance from each point. This operation will allow you to fill the blanks in a dashed lane
line.
![image](https://user-images.githubusercontent.com/105145104/168330768-c9d8b7b3-e57b-4041-946a-4deb21b19e89.png)

