# Computer Vision Based System for Automated Water Meter Reading  

**Authors**: Theodore J. Austria and Karl Vincent M. Espiritu  
**Undergraduate Project Advisor**: Nestor Michael Tiglao, PhD  
  
This is a project for our CoE 198 class, where we make use of an IoT, and Deep Learning Models. We use a module named ESP32-CAM to capture images of a water meter. This image is then inputted into our system of deep learning models. The output should display the predicted reading, and an estimated water bill for the household.  
  
To read more regarding this project, you may take a look at our Documentation of the project [here](https://github.com/espiritukarl/CoE-198/blob/main/Final%20Documentation.pdf).  
If you want to take a look at the source codes used, you may refer to [this](put_source_code_folder). The main file can be seen [here](https://github.com/espiritukarl/CoE-198/blob/main/watermeter_prediction.ipynb).  
  
The source codes folder contains:  
-
-
- 
-

## Project Flow
The process of the project can be seen in the flow chart below. ![project flowchart](https://github.com/espiritukarl/CoE-198/blob/main/images/flowchart.png?raw=true)  
  
First, the ESP32-CAM Module captures an image of a water meter. Our system then automatically draws a bounding box on its Region of Interest (ROI), in this case the digits for its water consumption. ![watermeters roi w/ bounding box](https://github.com/espiritukarl/CoE-198/blob/main/images/boundingbox_automated.PNG?raw=true) Then the ROI is isolated by putting a mask everywhere else, and then the mask is cropped. ![masked roi watermeter](https://github.com/espiritukarl/CoE-198/blob/main/images/roi_masked.PNG?raw=true) ![cropped roi watermeter](https://github.com/espiritukarl/CoE-198/blob/main/images/roi_cropped.PNG?raw=true)

## Step-by-Step Guide

### I. Materials
- Jumper wires  
- ESP32-CAM Module  
- FTDI Adapter  

### II. Setting up Google Script
To fully prepare the things needed for coding, we can do this step first. We need a program that can receive the image and store it in your Google Drove. Here are the steps (taken from this [website](https://www.gsampallo.com/2019/10/13/esp32-cam-subir-fotos-a-google-drive/)):  
1. Go to your Google Drive. Click New -> More -> Google Apps Script.
2. Copy and paste the code in the .js file to the text editor. Name your project with something like "ESP32"
3. You need to publish the script. To do that click on Deploy -> New Deployment.
