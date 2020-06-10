<img src="https://github.com/Godson-Thomas/Colour_Detection-Python/blob/master/Files/detected.gif" width="400"  /> <br><br>
# IMAGE PROCESSING
Image processing is a method to perform some operations on an image, in order to get an enhanced image or to extract some useful information from it.The input of that system is a digital image and the system process that image using efficient algorithms, and gives an image as an output.


## Libraries Used :
- OpenCV
- Pandas
- Numpy
# Steps
## Installation


1. We will be using Jupyter Notebook for writing the code.Make sure you have Jupyter Notebook installed.<br><br>
2. Lauch your Jupyter Notebook<br><br>
3. Now we have to install the OpenCV library.Type the code one by one in the cell of Jupyter Notebook and run it.
```
pip install pandas
```
```
pip install opencv-python
```
```
pip install numpy
```
<br><br>
* ## Code 
  - Full code.   [click here]()<br><br>
4. Import the modules
```
import cv2
import numpy as np
import pandas as pd
import argparse
```
<br>

5. Create an argument parser to take image path from command line
```
ap = argparse.ArgumentParser()
ap.add_argument('-i', '--image', required=True, help="Image Path")
args = vars(ap.parse_args())
img_path = args['image']
```
<br>

6. Read the image.   [sample image](https://raw.githubusercontent.com/Godson-Thomas/Colour_Detection-Python/master/Files/pic.jpg)
```
img = cv2.imread("/home/color-detection/pic.jpg")
```
<br>

7. Declaring global variables to be used
```
clicked = False
r = g = b = xpos = ypos = 0
```
<br>

8. Reading csv file and assigning names to each column.  [csv file](https://raw.githubusercontent.com/Godson-Thomas/Colour_Detection-Python/master/Files/colours.csv)
```
index=["color","color_name","hex","R","G","B"]
csv = pd.read_csv("/home/color-detection/colours.csv", names=index, header=None)
```
<br>

9. Defining a function to calculate minimum distance from all colours and finding the most appropriate colour.
```
def getColorName(R,G,B):
    minimum = 10000
    for i in range(len(csv)):
        d = abs(R- int(csv.loc[i,"R"])) + abs(G- int(csv.loc[i,"G"]))+ abs(B- int(csv.loc[i,"B"]))
        if(d<=minimum):
            minimum = d
            cname = csv.loc[i,"color_name"]
    return cname
```
<br>

10. Function to get x,y coordinates on double clicking mouse
```
def draw_function(event, x,y,flags,param):
    if event == cv2.EVENT_LBUTTONDBLCLK:
        global b,g,r,xpos,ypos, clicked
        clicked = True
        xpos = x
        ypos = y
        b,g,r = img[y,x]
        b = int(b)
        g = int(g)
        r = int(r)
```
<br>

11.Displaying colour name and RGB values
```
cv2.namedWindow('image')
cv2.setMouseCallback('image',draw_function)

while(1):

    cv2.imshow("image",img)
    if (clicked):
   
        
        cv2.rectangle(img,(20,20), (750,60), (b,g,r), -1)

       
        text = getColorName(r,g,b) + ' R='+ str(r) +  ' G='+ str(g) +  ' B='+ str(b)
        
       
        cv2.putText(img, text,(50,50),2,0.8,(255,255,255),2,cv2.LINE_AA)

       
        if(r+g+b>=600):
            cv2.putText(img, text,(50,50),2,0.8,(0,0,0),2,cv2.LINE_AA)
            
        clicked=False

    
    if cv2.waitKey(20) & 0xFF ==27:
        break
    
cv2.destroyAllWindows()
```
<br><br>

- Creating a string to display RGB values and Colour name
```
        text = getColorName(r,g,b) + ' R='+ str(r) +  ' G='+ str(g) +  ' B='+ str(b)
```

- We will display the text in black colour for lighter colours
```
        if(r+g+b>=600):
            cv2.putText(img, text,(50,50),2,0.8,(0,0,0),2,cv2.LINE_AA)
            
        clicked=False
```
- Breaking the loop when user hits 'esc' key
```    
    if cv2.waitKey(20) & 0xFF ==27:
        break
    
cv2.destroyAllWindows()
```