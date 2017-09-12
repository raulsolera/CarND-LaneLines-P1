# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./pipeline_progress_images/1_gray_image.jpg "Grayscaled"
[image2]: ./pipeline_progress_images/2_blurred_image.jpg "Blurred"
[image3]: ./pipeline_progress_images/3_edges_image.jpg "Edges"
[image4]: ./pipeline_progress_images/3_masked_edges_image.jpg "Region limited edges"
[image5]: ./pipeline_progress_images/4_hough_lines_image.jpg "Hough lines"
[image6]: ./pipeline_progress_images/4_average_hough_line_image.jpg "Average Hough lines"
[image7]: ./pipeline_progress_images/4_masked_averaged_image.jpg "Region limited average Hough lines"
[image8]: ./pipeline_progress_images/5_weighed_lined_image.jpg "Final result"


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:
- Convert the images to grayscale
- Apply a gaussian blur
- Define the region of interest (in which the lines are)
- Use canny algorithm for edge detection: firt getting full image edges and then limited to the region of interest ()
- Apply the hough transformation to detect the lines formed by the edges: first getting the raw lines, separating them between left and right lines and averaging both groups and finally drawing the averaged lines in the region of interest
- Superimpose the lines

I do not use the color detection technique as I consider it may cause detection errors in case of different color lines (ie. yellow lines) or changing light / shadow conditions.

In order to draw a single line on the left and right lanes, instead of using the openCV HoughLinesP function which return the image with the detected lines drawn on it I used the function HoughLines that returns all the lines detected in rho / theta form.

Then I process all the lines and separate left and rigth lines based on the angle theta (this also allows to discard some lines like the ones detected in challenge video due to the front of the car being in the camera field) and average left and right lines to obtain two single lines.

The pipeline proceeds as follows:

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be that the hough lines left / rigth discrimination is based on the angle and this may discard right lines or keep wrong ones.

Another shortcoming could be the averaging over the hough lines that may not get the best fitting line as it can be seen in the left line of challenge video output.

Finally some test should be done for images in close curves or steep slopes to check if the pipeline is affected by this condition.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to adjust the camera vision field depending on the vehicle in which is mounted to avoid the front of the car in the camera vision field.

This issue occur in the challenge video that require some extra check to get the right Hough lines. This can be adressed in two ways:
1. define a region of interest that lefts the car front outside.
2. reject the hough lines obtained if they fall out certain theta values (in my experience Hough lines for the lane lines should be close to theta values of 3/4*pi and 1/4*pi).

Another potential improvement could be to improve the averaging function to get a better fit to the lines which is quite good in the simple video but worse in the challenge video.
