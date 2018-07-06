# RoverProject
Wrote a computer vision pipeline (using Python and OpenCV) to perform color thresholding, as well as perspective and coordinate transforms to complete the task of autonomous mapping and navigation in a simulated (Unity) environment.

## Project: Search and Sample Return



**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook).
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands.
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

---
### __Writeup / README__

You're reading it!

Hi. I am Miguel Colmenares, engineer in electronics and i want to tell you about my experience in this project.

### __Notebook Analysis__
Analysis of the file named `Rover_Project_Test_Notebook` con formato [ipynb - html - PDF]

The first step is to convert the image into an array to edit it.

![alt text](https://github.com/Miguelucho/RoverProject/blob/master/test_notebook/imgnro5.png)

The next step is to implement the following functions to determine the values ​​and ranges of work for Rover.

![alt text](https://github.com/Miguelucho/RoverProject/blob/master/test_notebook/imgcalibration.png)

With this image with the terrain grid, I can identify the four points that create a square meter and in this way implement a transformation of perspective with the function: `perspect_transform` and get this output.

![alt text](https://github.com/Miguelucho/RoverProject/blob/master/test_notebook/imgperspective.png)

Now to identify the terrain where rover can drive we rely on the color of the floor which will seep into the image with a color threshold, defined in the function `color_thresh` and get this output.

![alt text](https://github.com/Miguelucho/RoverProject/blob/master/test_notebook/imgthresholding.png)

Then, from the black and white image the rover coordinates are obtained and with them a distance and steering angle is obtained for rover. Values ​​that are achieved by implementing the functions: `rover_coords` and `to_polar_coords`.

To locate the yellow samples also applies a filter with a color threshold (yellow, RGB: 255,255,0). This is developed in function `find_rocks`

![alt text](https://github.com/Miguelucho/RoverProject/blob/master/test_notebook/imgsample.png)

Now all these functions are implemented in a single function named `process_image()`.

```
idx = np.random.randint(0, len(img_list)-1)
# idx get a random number.

image = mpimg.imread(img_list[idx])
# image get a random image.

warped, mask = perspect_transform(image, source, destination)
# warped get the transformed perspective.
# mask get gets the vision range of rover.
# souce = [[14, 140], [301 ,140],[200, 96], [118, 96]].
# destination get the square meter of terrain.

threshed = color_thresh(warped)
# threshed is the warped image filtered by the threshold.

xpix, ypix = rover_coords(threshed)
# xpix and ypix are the coordinates of rover.

dist, angles = to_polar_coords(xpix, ypix)
# dist is the distance that can be traversed rover in observable terrain.
# angles is the steering angle for rover.

```

This function processes the image taken by rover and gives the following result.

![alt text](https://github.com/Miguelucho/RoverProject/blob/master/test_notebook/imgcoordinate.png)
`Steering angle = 0.0514070222412 `

To process the cvs file, it uses the functions of the panda library with which it creates a Databucket () class that assigns the values ​​of the cvs file to a class variable.

```
class Databucket():
    def __init__(self):
        self.images = csv_img_list  
        self.xpos = df["X_Position"].values
        self.ypos = df["Y_Position"].values
        self.yaw = df["Yaw"].values
        self.count = 0
        self.worldmap = np.zeros((200, 200, 3)).astype(np.float)
        self.ground_truth = ground_truth_3d
```

Also in the notebook there is an example to create a video from the images contained in the IMG folder.

```
# Import everything needed to edit/save/watch video clips
from moviepy.editor import VideoFileClip
from moviepy.editor import ImageSequenceClip

# Define pathname to save the output video
output = '../output/test_mapping.mp4'
data = Databucket() #
clip = ImageSequenceClip(data.images, fps=60)

new_clip = clip.fl_image(process_image)
```

### __Autonomous Navigation and Mapping__

The rover's performance is linked to four files *python*.

 1. `drive_rover.py` : This file initialize all variables needed for rover performance.

 2. `supporting_functions.py` : This file contains help functions such as a function to convert to float `convert_to_float()`, a function to update to rover `update_rover()` and a function to create an output image  `create_output_images()`.

 3. `perception.py` :  This file contains the functions studied in the notebook which are responsible for the processing of the images captured by rover. Here the important thing is to assign the threshold values for the floor and the yellow rock as well as the values that define the square meter of the terrain.

 4. `decision.py` : This file is the most important. Here is the code that will tell rover what to do with the captured information.

After editing the `perception.py` and `decision.py` files the rover simulation is executed in mars in autonomous mode. The simulation runs with a screen resolution of 1152 x 720 and *Good* graphics quality.

To watch a video of the simulation you can find it on my YouTube channel: [My Rover in Mars](https://www.youtube.com/watch?v=Z5XWu1cTNi8&t=226s)

### __Result__

Rover navigates and explores the terrain effectively. Locate the yellow rocks. Avoid obstacles and walls well, and get a good fidelity of the terrain.

#### Pending Tasks

* Upload the project to github.
* Run the simulation on a machine with more resources to get better results.
* Make rover better avoid wall and obstacles
* Improve navigation
* Make rover look for yellow rocks
* Lift a hardware version of the project

![alt text](https://github.com/Miguelucho/RoverProject/blob/master/test_notebook/imgcontunua.png)
