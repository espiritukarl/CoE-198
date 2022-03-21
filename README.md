# Computer Vision Based System for Automated Water Meter Reading  

**Authors**: Theodore J. Austria and Karl Vincent M. Espiritu  
**Undergraduate Project Advisor**: Nestor Michael Tiglao, PhD  
  
This is a project for our CoE 198 class, where we make use of an IoT, and Deep Learning Models. We use a module named ESP32-CAM to capture images of a water meter. This image is then inputted into our system of deep learning models. The output should display the predicted reading, and an estimated water bill for the household. In this repository, we show the [project flow](https://github.com/espiritukarl/CoE-198#project-flow) and the [project step-by-step](https://github.com/espiritukarl/CoE-198#step-by-step-guide).
  
## Project Flow
Included in the project flow is the:
- [ROI Detection model](https://github.com/espiritukarl/CoE-198#roi-detection-model)
- [Digit Detection model](https://github.com/espiritukarl/CoE-198#digit-detection-model)
- [Digit Reading model](https://github.com/espiritukarl/CoE-198#digit-reading-model)
- [IoT Dashboard](https://github.com/espiritukarl/CoE-198#iot-dashboard)
- [Sample input](https://github.com/espiritukarl/CoE-198#sample-input)
- [Additional notes](https://github.com/espiritukarl/CoE-198#additional-notes)  

The process of the project can be seen in the flow chart below.  
![project flowchart](https://github.com/espiritukarl/CoE-198/blob/main/images/flowchart.png?raw=true)   
  
### ROI Detection model
First, the ESP32-CAM Module captures an image of a water meter. Our system then automatically draws a bounding box on its Region of Interest (ROI), in this case the digits for its water consumption. ![watermeters roi w/ bounding box](https://github.com/espiritukarl/CoE-198/blob/main/images/boundingbox_automated.PNG?raw=true)
  
Then the ROI is isolated by putting a mask everywhere else, and then the mask is cropped. ![masked roi watermeter](https://github.com/espiritukarl/CoE-198/blob/main/images/roi_masked.PNG?raw=true) ![cropped roi watermeter](https://github.com/espiritukarl/CoE-198/blob/main/images/roi_cropped.PNG?raw=true)
 
### Digit Detection model
The system now draws a bounding box around each digit of the ROI, then puts a mask around each digit. ![masked digits](https://github.com/espiritukarl/CoE-198/blob/main/images/digits_masked.PNG?raw=true)
  
Then each image's mask is then cropped out and then the digits are sorted.  
![cropped digits](https://github.com/espiritukarl/CoE-198/blob/main/images/digits_cropped.PNG?raw=true)
  
### Digit Reading model
They are then fed into the Digit Reading model, where the model predicts a label for each digit. They are then concatenated to each other to create a predicted reading. Seen below is also a sample of random digits with their predicted label.  
![concatenated digits](https://github.com/espiritukarl/CoE-198/blob/main/images/digits_final.PNG?raw=true)  
![predicted digits](https://github.com/espiritukarl/CoE-198/blob/main/images/predicted_digits.png)
  
### IoT Dashboard
The reading, together with its estimated water bill breakdown, is then sent to the IoT Dashboard. The MQTT broker used for this project is [things.ph](https://things.ph/). ![iot dashboard](https://github.com/espiritukarl/CoE-198/blob/main/images/iot_dashboard.PNG?raw=true)
  
### Sample input
An example of the input and output of the system can be seen here.  
![watermeter](https://github.com/espiritukarl/CoE-198/blob/main/images/watermeter.jpg?raw=true) ![watermeter results](https://github.com/espiritukarl/CoE-198/blob/main/images/watermeter_results.PNG?raw=true)
  
### Additional notes
To read more regarding this project, you may take a look at our Documentation of the project [here](https://github.com/espiritukarl/CoE-198/blob/main/Final%20Documentation.pdf).  
The main file of the code can be seen [here](https://github.com/espiritukarl/CoE-198/blob/main/watermeter_prediction.ipynb).  
Additionally, the source codes have also been uploaded [here](https://github.com/espiritukarl/CoE-198/tree/main/Source%20Codes). Included in this directory are:
- ROI Detection and Digit Detection model [(1)](https://github.com/espiritukarl/CoE-198/blob/main/Source%20Codes/Train_Mask_RCNN.ipynb)
- Digit Reading model [(2)](https://github.com/espiritukarl/CoE-198/blob/main/Source%20Codes/Train%20-%20Digit%20Reading.ipynb)
- Accuracy test [(3)](https://github.com/espiritukarl/CoE-198/blob/main/Source%20Codes/Accuracy.ipynb)
- F1 score test [(4)](https://github.com/espiritukarl/CoE-198/blob/main/Source%20Codes/F1%20Score.ipynb)

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
![setting up google script (3)](https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/gdrive1.PNG)
4. Click on the gear icon and choose Web App as the deployment type. Type in some description of the project. More importantle, choose "Anyone" for the "Who has access" part. Click "Deploy".  
![setting up google script (4)](https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/gdrive2.PNG)
5. After that, the Deployment ID and Web App URL will be given. Make sure to copy the URL as this will be needed for the Arduino code later on. The URL will have the following format: https://script.google.com/macros/s/XXXXXXXXXXXXXX/exec  
![setting up google script (5)](https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/gdrive3.PNG)
6. We are now ready to program our ESP32-CAM Module.

### III. Setting up the ESP32-CAM Module
In this part, we will be setting up the ESP32-CAM Module. This will be the module to be used for capturing photos of your water meters. You will need the materials stated above. For the software, we will be using Arduino IDE. All files used can be found [here](https://github.com/espiritukarl/CoE-198/tree/main/ESP32_Files).  
A. Installing ESP32 board in the Arduino IDE
1. In your Arduino IDE, go to File -> Preferences.
2. Type in “https://dl.espressif.com/dl/package_esp32_index.json, http://arduino.esp8266.com/stable/package_esp8266com_index.json” in the “Additional Boards Manager URLs:” field then click “OK”. ![setting up the esp32-cam module (2)]https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/esp1.PNG
3. Go to Tools -> Board -> Boards Manager.
4. Type in “esp32” in the search bar and install the board “ESP32 by Espressif Systems”. The figure below shows “INSTALLED” because we have installed the board previously. But if not, there should be an “Install” button for you.  
![setting up the esp32-cam module (4)](https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/esp2.PNG)
  
B. Programming the ESP32-CAM Module  
- Download the “esp32_files” from our github and make sure the files are in the same folder before uploading the code into the ESP32-CAM.
  
C. Wiring the ESP32-CAM Module  
After installing the board, we are now ready to connect the components together.
1. Using the materials stated [above](https://github.com/espiritukarl/CoE-198#i-materials), wire them up using the diagram below. Diagram taken from https://dronebotworkshop.com/esp32-cam-intro/. 
2. Notes: The connection between GPIO 0 and GND is required when uploading programs into the ESP32-CAM module. In order to test the program, remove the connection between those two pins.  
![wiring the esp32-cam module](https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/esp32.png)

### IV. Setting up MQTT Broker for IoT Dashboard  
Next is to set up the MQTT Broker for the IoT Dashboard. For our project, we will use the [things.ph](https://things.ph/) website. Go to “things.ph” and register for an account. There, the necessary details for your MQTT connection will be given.  
![setting up mqtt broker for iot dashboard](https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/things.PNG)

### V. Running the Code
1. For the code, we used Google Colab. Open the [notebook](https://github.com/espiritukarl/CoE-198/blob/main/watermeter_prediction.ipynb) and run the cells.
2. Run the Prerequisites section.  
![running the code (2)](https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/code1.PNG)
3. Mount your Google Drive.  
![running the code (3)](https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/code2.PNG)
4. Use the correct directory of the [Prediction_Files folder](https://github.com/espiritukarl/CoE-198/tree/main/Prediction_Files)  
![running the code (4)](https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/code3.PNG)
5. Load the models.
6. Run the functions. You can see all the functions below the “FUNCTIONS” text.
7. Run the code below the load model functions. Make sure there is an input image already.  
![running the code (7)](https://github.com/espiritukarl/CoE-198/blob/main/images/figures_sbs/code4.PNG)
