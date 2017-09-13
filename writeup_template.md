# **Finding Lane Lines on the Road** 

## Project 1 by Luke Raus

### Below describes the methods and logic I use to find the lanes lines.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 
1. I converted the images to grayscale
2. Defined a kernel size and apply Gaussian smoothing (I chose a kernal size of 5)
3.  Defined parameters for Canny and apply to the gray smooth image, which I choose a low threshold of 200 and a high threshold of 300
4. Defined a four sided polygon as an interest area in the image and blacked out all the areas outside that polygon of interest
5. Defined the Hough transform parameters to find all the points where lines intersect, and then applyed them to the image
6.  Finally I took the resulting sets of lines form Hough tranform, and applied some math in my draw lines function to extrapulate the 2 lines I needed to draw on the image.


In order to draw a single line on the left and right lanes, 
- I modified the draw_lines() function by first calucating the slopes of each line and seprating them into a left_slopes list and right_slopes list.
- I was only intested in slopes from 0.4 -> 0.9 for the left line and -0.4 -> -0.9 for the right line as those were the most likly to be the lane lines.  
- I also did not accept slopes of 0.0 (vertical lines).  
- Then I calculated the intercepts (y1 - slope*x1) of the lines I accepted to be used for later.  
- After I gathered my list of left and right slopes and intercetps I calcuted the mean of the slopes and intercepts to get a single likely slope and intercept.
- I then extraputed the min and max x value I could have for the line I wanted to draw using this formual:
```
x_min_left = int((y_min - left_intercept)/left_slope) 
x_max_left = int((y_max - left_intercept)/left_slope))
```
- finally I drew the line now now my x min and max. The y_max is the just the height of the image, and the y_min was the lowest y point value I could determin from my selected points
```
 cv2.line(img, (x_min_left, y_min), (x_max_left, y_max), color, thickness)
 ```

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
