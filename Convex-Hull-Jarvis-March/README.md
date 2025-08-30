# Computing the Convex Hull of a Set of Points in $\mathbb{R}^{2}$
## Using the Jarvis march method

Running the script `convex_hull_jarvis_march_animate.py` opens a window and prompts the user to input points in the window by clicking. Upon pressing enter, the script uses the _Jarvis march_ algorithm to compute the convex hull of the points and plots the intermediate steps of this process to illustrate how it works.

The Jarvis march works by first ordering all the points lexographically (i.e. in order of increasing x-coordinate and also by y-coordinate in the case that the x-coordinates are equal)

>In the example gifs, the red crosses are the set of points whose convex hull we wish to find. The red dotted lines represent intermediate steps and the solid blue lines represent segments of the final convex hull.
