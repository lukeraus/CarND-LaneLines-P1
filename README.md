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
[challengImage]: ./test_images/challengFrame.jpg "Challenge Image"
[challengeImageOut]: ./test_images_output/challengFrame_line_edges.jpg "Challenge Image Out"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. 
1. I converted the images to grayscale
2. Defined a kernel size and applied Gaussian smoothing (I chose a kernal size of 5)
3.  Defined parameters for Canny and applied it to the gray smooth image, which I choose a `low_threshold` of 200 and a `high_threshold` of 300
4. Defined a four sided polygon as an interest area in the image and blacked out all the areas outside that polygon of interest
5. Defined the Hough transform parameters to find all the points where lines intersect, and then applyed them to the image. My chosen parameters are listed below
```
rho = 1               # distance resolution in pixels of the Hough grid
theta = np.pi/180     # angular resolution in radians of the Hough grid
threshold = 15        # minimum number of votes (intersections in Hough grid cell)
min_line_length = 20  # minimum number of pixels making up a line
max_line_gap = 40     # maximum gap in pixels between connectable line segments
```
6.  Finally I took the resulting sets of lines from Hough tranform, and applied some math in my draw lines function to extrapulate the 2 lines I needed and drew them on the original image.


In order to draw a single line on the left and right lanes, I modifued the draw_lines() function in the following way:
- first I calucated the slopes of each line and seprating them into a left_slopes list and right_slopes list.
- I was only intested in slopes from 0.4 -> 0.9 for the left line and -0.4 -> -0.9 for the right line as those were the most likly to be the lane lines.  
- I also did not accept slopes of 0.0 (vertical lines).  
- Then I calculated the intercepts `(y1 - slope*x1)` of the lines I accepted to be used for later.  
- After I gathered my list of left and right slopes and intercetps I calcuted the mean of the slopes and intercepts to get a single likely slope and intercept.
- I then extraputed the min and max x value I could have for the line I wanted to draw using this formual:
```
x_min_left = int((y_min - left_intercept)/left_slope) 
x_max_left = int((y_max - left_intercept)/left_slope))
```
- finally I drew the line with my x min and max. The y_max is the just the height of the image, and the y_min was the lowest y point value I could determin from my selected points
```
 cv2.line(img, (x_min_left, y_min), (x_max_left, y_max), color, thickness)
 ```

### 1.5 Notes

One step I took that really helped me find the best Canay and Hough line parametes is that I took a single frame from the callenge video:
![alt text][challengImage]

And tested my pipeline on this image till I got a result like this:
![alt text][challengeImageOut]  


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when it is night time.
It was not tested in the project but I am wondering how well my pipeline would work given a low light situation.  I would propably have to change the Canay paramters and the Hough lines transform parameters. 

Another shortcoming could be curved lane lines as we are only drawing straight lines in these examples we would need a way to find a cureved line given the lines produced from the Hough transformation lines.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to find a way to figure out the best paramerters for Canay and Hough transform functions programically.  Right now I just guessed at the best numbers till I got a good result.  But these were ideal lighting conditions.  Given a more diverse set of lighting and less defined lane lanes I would have to find a programic way to determine these parameters.

Another potential improvement could be to draw curved lines or the ability to identify if a lane line is curved.  
