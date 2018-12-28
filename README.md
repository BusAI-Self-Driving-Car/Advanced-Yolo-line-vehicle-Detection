# Advanced Lane Finding Project and Yolo Objection Detection.   
## Tabel of Content
1 - [Algorithm Details](#algorithm-details).  
2 - [YOLO Object Detection](#yolo-object-detection).  
3 - [Video Output](#video-output).  
4 - [Dependencies and Compiling](#dependencies-and-compiling).  
5 - [Tensorflow Static Build](#tensorflow-static-build).   
## Algorithm Details.     
### Distortion corrected calibration image.   
The code for this step is contained in the calibration.cpp [Here](../LaneandYolovehicle-Detection/calibration.cpp).  
Start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here we are assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image. Thus, objp is just a replicated array of coordinates, and objpoints will be appended with a copy of it every time I successfully detect all chessboard corners in a test image. imgpoints will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection. Then use the output objpoints and imgpoints to compute the camera calibration and distortion coefficients using the calibrateCamera() function.  
Example:   
![alt text]( images/image1.png)    
#### Pipeline.   
At First,  resize the image and then  convert frame as Bird view and then use a combination of color and gradient thresholds to generate a binary image.  
**Step 1:** Undisort Image.   
**Step 2:** Binary Image.   
**Step 3:** Take a histogram along all the columns in the lower half of the image and split histogram for two sides for each lane.   
**Step 4:** Use the two highest peaks from histogram as a starting point for determining where the lane lines are, and then use sliding windows moving upward in the image to determine where the lane lines go.   
Example: 
![alt text](images/image2.png)   
**Step 5:**  Identify lane-line pixels.    
Find all non zero pixels.  
Example:   
![alt text](images/image3.png)    
**Step 6:** Fit their positions with a polynomial.   
After performing 2nd order polynomial fit for nonzero pixels, drawing polyline and unwrap image the final output.  
Example:   
![alt text](images/image4.png)     
### Radius of curvature of the lane and the position of the vehicle with respect to center. 
Get the left and right cordinates and calculate the midpoint of lanes and use the image center as reference to calculate distance away from center.  
1-	LANEDETECTION::center_dist – Distance of Vechicle from center.  
2-	LANEDETECTION::left_curverad – Left Lane Curvature.  
3-	LANEDETECTION::right_curverad – Right Lane Curvature.  
## YOLO Object Detection.  
The Tensorflow Model is trained for five classes of objects: cars, pedestrians, truck, cyclists and traffic lights.  
Example:   
![alt text]( images/image5.png)    

## Video Output.  
The Video output can be found [Here]( https://github.com/apreddyy/LaneandYolovehicle-Detection/blob/master/out.avi).  
## Dependencies and Compiling. 
### Environment Windows 10.  
1-	Visual Studio 2015.  
2-	CUDA 9.0. For Windows installation Guide and Requirements [Here]( https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#install-cuda-software).   
3-	TBB . For more information and installation [Here]( https://software.intel.com/en-us/intel-tbb).   
4-	Tensorflow 1.7.0. For Building a static Tensorflow C++ library on Windows [Here]( https://joe-antognini.github.io/machine-learning/build-windows-tf) and in [Section](#tensorflow-static-build).    
5-	OpenCV 3.4 or Greater. More information can be found [Here]( https://jamesbowley.co.uk/build-compile-opencv-3-4-in-windows-with-cuda-9-0-and-intel-mkl-tbb/).    
6-	Tensorflow trained model (graph-vehicledetection.pb) is included in repository.  
7-            Eigen Library.  
## Tensorflow Static Build.
### Clone Tensorflow.  
git clone https://github.com/tensorflow/tensorflow.git v1.7.0  
cd v1.7.0  
git checkout tags/v1.7.0  
cd tensorflow\contrib\cmake  
mkdir build  
cd build  
### Build Tensorflow  
 Drive:\tensorflow\tensorflow\contrib\cmake .. -A x64 -DCMAKE_BUILD_TYPE=Release ^  
-DSWIG_EXECUTABLE=DRIVE:\PATH-To\Swig\swigwin-3.0.12\swig.exe ^  
-DPYTHON_EXECUTABLE=DRIVE:\Path-To\python.exe ^  
-DPYTHON_LIBRARIES=DRIVE:\Path-To\libs\python35.lib ^  
-Dtensorflow_ENABLE_GPU=ON ^  
-DCUDNN_HOME="C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0" ^  
-Dtensorflow_BUILD_PYTHON_BINDINGS=OFF ^  
-Dtensorflow_ENABLE_GRPC_SUPPORT=OFF ^  
-Dtensorflow_BUILD_SHARED_LIB=ON ^  
-Dtensorflow_WIN_CPU_SIMD_OPTIONS=/arch:AVX ^  
-DCUDA_HOST_COMPILER="C:/Program Files (x86)/Microsoft Visual Studio 14.0/VC/bin/amd64/cl.exe"  
-Dtensorflow_WIN_CPU_SIMD_OPTIONS=/arch:AVX  
# Thanks
