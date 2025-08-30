# Computing the Convex Hull of a Set of Points in $\mathbb{R}^{2}$
## Using the Jarvis march method

Running the script `convex_hull_jarvis_march_animate.py` opens a window and prompts the user to input points in the window by clicking. Upon pressing enter, the script uses the _Jarvis march_ algorithm to compute the convex hull of the points and plots the intermediate steps of this process to illustrate how it works.

The Jarvis march works by first ordering all the points lexographically (i.e. in order of increasing x-coordinate and also by y-coordinate in the case that the x-coordinates are equal) to create a list, points_list. One then considers the line segment between the first two points in points_list and checks all other points to see whether they lie to the left or to the right of the line segment. If all points lie to the right then the line segment is added to the convex hull. If any points lie to the left, it is discarded. One then considers the line segments between the last point in the convex hull and each other point in points_list and applies the same test.

>In the example gifs, the red crosses are the set of points whose convex hull we wish to find. The solid blue lines represent segments of the final convex hull. The solid black lines represent the line segment which is being tested for admission to the hull. The dotted lines represent intermediate steps which test which points lie to the left (red dotted) and right (green dotted) of the solid black line. If the solid black line is to be discarded, then it turns briefly into a black dashed line.
