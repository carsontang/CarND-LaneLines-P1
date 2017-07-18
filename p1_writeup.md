**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of four steps. The first step is to convert the image to grayscale. Then the edges in the image are detected with the Canny edge detection algorithm. Next, a triangular region is overlaid at the bottom of the image. The bottom two corners and the middle of the image make up the triangle. Pixels outside this triangle are filtered out. With only the edges of the lanes visible at this point, the lines are computed and overlaid onto the image.

The function `draw_lines` is used in the previous step to compute the lines. It's been modified to calculate the slope of each line. The x and y coordinates that make up the lines are separated into two different groups, where each group is assumed to be part of the left or right lanes. If the line is positively sloped, its points are assumed to be a part of the right lane. If the line is negatively sloped, its points are assumed to be part of the left lane. Points belonging to lines with a slope outside the range of acceptable slopes aren't added into any group.

Next, with the two groups of points, `numpy.polyfit` is used to find the best fit lines through the two groups. The resulting equations correspond to lines that fit through the left and right lanes.

If both lines are drawn on the image, then they'll create a big X across the image. Instead, only the bottom segment of each is drawn. 


### 2. Identify potential shortcomings with your current pipeline

The current pipeline assumes there are two groups of lines, the left lane line and the right lane line. If there are shadows of straight objects like bridges or other vehicles, the pipeline will group all positively sloped lines into one group and all negatively sloped lines into another. The average fit lines through the positive and the negative lines would not fit on the lanes.

Another shortcoming is the pipeline assumes the image or video shows nothing but the road and the objects ahead. If the vehicle's hood is in the image, its edges could ruin the best fit lines through the left and right lane lines.

Furthermore, if the horizon is way above the middle of the image or the video, the triangular mask won't capture most of the upper part of the lanes, which would prevent the pipeline from finding enough points to draw a line through them.

### 3. Suggest possible improvements to your pipeline

One possible improvement would be to cluster the lines detected in the image or video into groups with some clustering algorithm instead of naively assuming all lines that are positively sloped must belong to one lane and all lines that are negatively sloped must belong to the other. Then, from the clusters, choose the two lines that are most likely to be the lanes and use those lines to find the best fit left and right lanes.

Another improvement would be to filter out colors that aren't white or yellow to avoid having edges from shadows interfere with the best fit lines.