
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from matplotlib.animation import PillowWriter


fig, ax = plt.subplots(figsize=(6,6))
ax.tick_params(axis='both', which='both', bottom=False, left=False, 
               labelbottom=False, labelleft=False)
ax.set_xlim(0, 10)
ax.set_ylim(0, 10)
plt.title("Click points (Enter to finish)") 
points_clicked = plt.ginput(n=-1, timeout=0)  # unlimited clicks until Enter/middle click
plt.title('')
line1, line2, line3 = ax.plot([], [], 'b-', [], [], 'k-', [], [])

# Test if three points in R2 form a right-hand turn in the order they are listed
def is_right_turn(points):
    # The input must be a list of three 2d points
    if len(points) != 3:
        print("List must contain exactly three points")
    elif any([len(pt) != 2 for pt in points]):
        print("All points must be 2d")
    else:
        # test if p3 lies to the right of the line
        # p1p2 by using the cross product,
        # i.e. imagine all points as living in
        # a 2d subspace of R3.
        
        points = np.array(points)
        
        p1_to_p2 = points[1] - points[0]
        p1_to_p3 = points[2] - points[0]

        cross_prod_comp = (p1_to_p3[0]*p1_to_p2[1] - 
                           p1_to_p3[1]*p1_to_p2[0])

        if cross_prod_comp > 0:
            return True
        else:
            return False

# Compute the convex hull of a list of points in R2, output the convex hull
# as a the list of points forming the convex hull read clockwise. Also ouput
# the intermediate steps of the algorithm for the purpose of the animation.
def convex_hull_2d_jarvis(points):
    if len(points) < 3:
        print("There must be at least three points")
    else:
        # Sort the points lexographically
        points = sorted(points)
        hull = [points[0]]
        steps = []
        back_to_start = False
        while not back_to_start:
            for i in range(0, len(points)):
                if points[i] == hull[-1]:
                    continue
                if len(hull)>1 and points[i] == hull[-2]:
                    continue
                hull += [points[i]]
                steps += [[hull.copy()[:-1],hull.copy()[-2:], 0]]
                remaining_points = [pt for pt in points if (not pt in hull[-2:])]
                is_all_right_turns = [is_right_turn(hull[-2:]+[p]) 
                                      for p in remaining_points]
                for i in range(len(is_all_right_turns)):
                    steps += [[hull.copy()[:-1],hull.copy()[-2:], 2, 
                               is_all_right_turns[i], 
                               [hull.copy()[-1], remaining_points[i]]]]
                if not all(is_all_right_turns):
                    steps += [[hull.copy()[:-1],hull.copy()[-2:], 1]]
                    hull.pop(-1)
                    steps += [[hull.copy(),[], 3]]
                else:
                    steps += [[hull.copy(),[], 3]]
                    break
            if hull[0]==hull[-1]:
                back_to_start = True
    return hull, steps

x1, y1 = np.array(points_clicked).T
convex_hull_points, steps = convex_hull_2d_jarvis(points_clicked)
# Keep a list of all the lines in the convex hull, this allows to plot some lines
# as solid blue and some as red dashed in the animation, depending if they are in
# final convex hull or just represent intermediate steps.
# convex_hull_lines = []
# for i in range(len(convex_hull_points)-1):
#     convex_hull_lines.append([convex_hull_points[i], convex_hull_points[i+1]])
# convex_hull_lines.append([convex_hull_points[-1], convex_hull_points[0]])

# The initial state of the animation.
def init():
    line1.set_data([], [])
    line2.set_data([], [])
    line3.set_data([], [])
    return line1, line2, line3

# The function which updates each frame of the animation.
def update(frame):
    ax.plot(x1,y1,'r+') 
    hull_temp = steps[frame]
    # This plots solid blue lines that are in the hull and plots solid black lines
    # that are being tested
    if hull_temp[2] == 0: 
        if len(hull_temp[0])==1:
            line2data = np.array(hull_temp[1])
            line1.set_data([],[])
            line2.set_linestyle('-')
            line2.set_data(line2data[:,0], line2data[:,1])
            line3.set_data([],[])
        else:
            line1data = np.array(hull_temp[0])
            line2data = np.array(hull_temp[1])
            line1.set_data(line1data[:,0], line1data[:,1])
            line2.set_linestyle('-')
            line2.set_data(line2data[:,0], line2data[:,1])
            line3.set_data([],[])
    # This plots solid blue lines that are in the hull and turns the solid black line
    # to a dashed black line if it is to be removed
    elif hull_temp[2] == 1:
        if len(hull_temp[0])==1:
            line3data = np.array(hull_temp[1])
            line1.set_data([],[])
            line2.set_linestyle('--')
            line2.set_data(line3data[:,0], line3data[:,1])
            line3.set_data([],[])
        else:
            line1data = np.array(hull_temp[0])
            line3data = np.array(hull_temp[1])
            line1.set_data(line1data[:,0], line1data[:,1])
            line2.set_linestyle('--')
            line2.set_data(line3data[:,0], line3data[:,1])
            line3.set_data([],[])
    # This plots solid blue lines that are in the hull and plots solid black lines
    # that are being tested and plots all the intermediate lines that are used to test
    # if the solid black line should be kept. Green dotted lines indicate points lying to the 
    # right of the black line and red dotted are those to the left.
    elif hull_temp[2] == 2:
        if len(hull_temp[0])==1:
            line2data = np.array(hull_temp[1])
            line1.set_data([],[])
            line2.set_linestyle('-')
            line2.set_data(line2data[:,0], line2data[:,1])
            if hull_temp[3]==True:
                line3data = np.array(hull_temp[4])
                line3.set_color('green')
                line3.set_linestyle(':')
                line3.set_linewidth(3)
                line3.set_data(line3data[:,0], line3data[:,1])
                plt.title('')
            else:
                line3data = np.array(hull_temp[4])
                line3.set_color('red')
                line3.set_linestyle(':')
                line3.set_linewidth(3)
                line3.set_data(line3data[:,0], line3data[:,1])
                plt.title('Remove line segment')
        else:
            line1data = np.array(hull_temp[0])
            line2data = np.array(hull_temp[1])
            line1.set_data(line1data[:,0], line1data[:,1])
            line2.set_linestyle('-')
            line2.set_data(line2data[:,0], line2data[:,1])
            if hull_temp[3]==True:
                line3data = np.array(hull_temp[4])
                line3.set_color('green')
                line3.set_linestyle(':')
                line3.set_linewidth(3)
                line3.set_data(line3data[:,0], line3data[:,1])
                plt.title('')
            else:
                line3data = np.array(hull_temp[4])
                line3.set_color('red')
                line3.set_linestyle(':')
                line3.set_linewidth(3)
                line3.set_data(line3data[:,0], line3data[:,1])
                plt.title('Remove line segment')
    # Plots the current state of the convex hull
    else:
        line1data = np.array(hull_temp[0])
        line1.set_data(line1data[:,0], line1data[:,1])
        line2.set_data([],[])
        line3.set_data([],[])
    return line1, line2, line3

ani = FuncAnimation(fig=fig, 
                          func=update, 
                          init_func=init,
                          frames=len(steps),
                          interval=200, 
                          repeat=False)

# #To save the animation using Pillow as a gif
# writer = PillowWriter(fps=2,
#                       metadata=dict(artist='Me'),
#                       bitrate=1800)
# ani.save('convex_hull3.gif', writer=writer)

plt.show()
