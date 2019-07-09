# CarND-Advanced-Lane-Lines

The code for this step is contained in the second code cell of the IPython notebook located in “CarND-Advanced-Lane-Lines" 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image. The chessboard grid size is 9 rows and 6 cols. Thus, objp is just a replicated array of coordinates, and objpoints will be appended with a copy of it every time I successfully detect all chessboard corners in a test image. Imgpoints will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

# Step 1 : Calibrate Images
I then used the output objpoints and imgpoints to compute the camera calibration and distortion coefficients using the cv2.calibrateCamera() function. I applied this distortion correction to the test image using the cv2.undistort () function and obtained this result:  

# Step 2: Apply a distortion correction to raw images
To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one. We use the camera matrix, distortion coefficients and use cv2.undistort() to apply on the raw images and result is below.  

# Step3: Use color transforms, gradients, etc., to create a threshold binary image.

From code cell 6 there are 2 functions that performs the following actions to create a binary image
1. **Sobel_operation**  
Converting the original image to gray, I applied open CV’s sobel operator in the X direction to obtain the following result.

2. **hls_operation**
Using open CV “COLOR_RGB2HLS” converted the source image into HLS color space.


Now the result of these 2 functions is combined to get the following binary image.


# Step 4.	Apply a perspective transform to rectify binary image ("birds-eye view").
From code cell 6 the function getBinaryWarpedImage takes the binary image along with SOURCE and DESTINATION points as stated below, along with MATRIX obtained by calibration camera in step 1 to produce a binary warped image.
  src = np.float32([[210, 710],[600,453],[680, 453],[880,710]]) 
  dst = np.float32([[300, 710],[300, 0],[880, 0],[880,710]])
This is hardcoded for all images and obtained after a result of trial and error on all the test images. I veriﬁed that my perspective transform was working as expected by drawing the src and dst points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.


# Step 5 Detect lane pixels and fit to find the lane boundary, identify ROC calculation and the position of the vehicle with respect to center in the lane

In code cell 18 fitPolynomial function accept the binary warped image and fits the polynomial line.


It’s divided into 3 major sections separated by “##########################”
The top section calls 2 functions  
- Find Lanes – Sliding window search to find the lanes in a binary warped images and get the coefficient of polynomial, the positions of the both the lanes. This is used in the first image only and in the images that has “Shades of tree”. Reasons explained the last section of the document.  
- getLaneInfo –  The “look ahead” filter that uses the co-efficient from previous step and fits the polynomial returning the new coefficients  and the positions of the both the lanes.  

The middle section uses the coefficients from the previous section and calculates the Radius of curvature in m and position of the vehicle with respect to center in the lane. You can find them in the section marked by the below comments.
