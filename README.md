# SFND 2D Feature Tracking

<img src="images/keypoints.png" width="820" height="248" />

The idea of the camera course is to build a collision detection system - that's the overall goal for the Final Project. As a preparation for this, you will now build the feature tracking part and test various detector / descriptor combinations to see which ones perform best. This mid-term project consists of four parts:

* First, you will focus on loading images, setting up data structures and putting everything into a ring buffer to optimize memory load. 
* Then, you will integrate several keypoint detectors such as HARRIS, FAST, BRISK and SIFT and compare them with regard to number of keypoints and speed. 
* In the next part, you will then focus on descriptor extraction and matching using brute force and also the FLANN approach we discussed in the previous lesson. 
* In the last part, once the code framework is complete, you will test the various algorithms in different combinations and compare them with regard to some performance measures. 

See the classroom instruction and code comments for more details on each of these parts. Once you are finished with this project, the keypoint matching part will be set up and you can proceed to the next lesson, where the focus is on integrating Lidar points and on object detection using deep-learning. 

## Dependencies for Running Locally
1. cmake >= 2.8
 * All OSes: [click here for installation instructions](https://cmake.org/install/)

2. make >= 4.1 (Linux, Mac), 3.81 (Windows)
 * Linux: make is installed by default on most Linux distros
 * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
 * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)

3. OpenCV >= 4.1
 * All OSes: refer to the [official instructions](https://docs.opencv.org/master/df/d65/tutorial_table_of_content_introduction.html)
 * This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors. If using [homebrew](https://brew.sh/): `$> brew install --build-from-source opencv` will install required dependencies and compile opencv with the `opencv_contrib` module by default (no need to set `-DOPENCV_ENABLE_NONFREE=ON` manually). 
 * The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)

4. gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using either [MinGW-w64](http://mingw-w64.org/doku.php/start) or [Microsoft's VCPKG, a C++ package manager](https://docs.microsoft.com/en-us/cpp/build/install-vcpkg?view=msvc-160&tabs=windows). VCPKG maintains its own binary distributions of OpenCV and many other packages. To see what packages are available, type `vcpkg search` at the command prompt. For example, once you've _VCPKG_ installed, you can install _OpenCV 4.1_ with the command:
```bash
c:\vcpkg> vcpkg install opencv4[nonfree,contrib]:x64-windows
```
Then, add *C:\vcpkg\installed\x64-windows\bin* and *C:\vcpkg\installed\x64-windows\debug\bin* to your user's _PATH_ variable. Also, set the _CMake Toolchain File_ to *c:\vcpkg\scripts\buildsystems\vcpkg.cmake*.


## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./2D_feature_tracking`.

# Writeup

## 1. Data Buffer

### MP.1 Data Buffer Optimization
* The ring buffer idea is to have a fixed data buffer of a specific size n and when a new element is ready to be added to the buffer.<br/>

* If the current data buffer size is less than the size n, the element will be added to the buffer.<br/>

* Else if the current size of the data buffer = n the first element in the buffer will be removed, all the elements in the buffer shift to left then the new element is added to the end of the buffer.<br/>

* That ensures the size of the buffer doesn't exceed the predefined buffer size n and it has the last n element that arrived. 
<br/>


##  2. Keypoints

### MP.2 Keypoint Detection <br/>

* Different Keypoint detection algorithms have been implemented : 
   * SHITOMASI 
   * HARRIS
   * FAST
   * BRISK
   * ORB
   * AKAZE
   * SIFT <br/>
* One can choose which method to apply by changing the string variable `detectorType = "a name form the list"` 
<br/>

### MP.3 Keypoint Removal<br/>

* Keep the keypoints that are only located inside a predefined rectangle.<br/>
* `rect.contains()` method is used to filter out all keypoints outside the predefined rectangle. <br/> 

## 3. Descriptors<br/>

### MP.4 Keypoint Descriptors <br/>
* Different Keypoint descriptors algorithms have been implemented :
   * BRISK
   * BRIEF
   * ORB
   * FREAK
   * AKAZE
   * SIFT<br/>
* One can choose which method to apply by changing the string variable `descriptorType = "a name form the list"` <br/>
### MP.5 Descriptor Matching<br/>
* Different Descriptor Matching algorithms have been implemented :
   * MAT_BF   &emsp; &emsp; &emsp;     BrutForceMatcher
   * MAT_FLANN   &emsp;    &nbsp;  FlannBasedMatcher
    <br/>
* Different selection Matching type have been implemented :
   * SEL_NN      &emsp;   &emsp; &emsp;   Nearest neighbor (best match)
   * SEL_KNN     &emsp;  &emsp;  &nbsp;   k nearest neighbors (k=2)

* One can choose which method to apply by changing the string variables `matcherType` and `selectorType`   <br/>

### MP.6 Descriptor Distance Ratio <br/>
 * The descriptor distance ratio test  has been implemented. It works as a filtering method to remove bad keypoint matches.<br/>


## 4. Performance<br/>

### MP.7 Performance Evaluation 1 <br/>

The the number of keypoints on the preceding vehicle for all 10 images and of avg neighborhood size for all the detectors we have implemented.<br/> 

Detector | imag 0 | imag 1 | imag 2 | imag 3 | imag 4 | imag 5 | imag 6 | imag 7 | image  | imag 9 | Avg Neighborhood size | Time in ms
| :---:    | :---:  | :---:  | :---:  |  :---: | :---:  | :---:  | :---:  | :---:  | :---:  | :---:  | :---: | :---: |
| SHI-TOMASI | 125 | 118 | 123 | 120 | 120 | 113 | 114 | 123 | 111 | 112 | 4 | 100 ms
| HARRIS | 17 | 14 | 18 | 21 | 26 | 43 | 18 | 31 | 26 | 34 | 6 | 115 ms
| FAST | 149 | 152 | 150 | 155 | 149 | 149 | 156 | 150 | 138 | 143 | 7 | 12 ms
| BRISK | 264 | 282 | 282 | 277 | 297 | 279 | 289 | 272 | 266 | 254 | 20.5 - 22.6 | 324 ms
| ORB | 92 | 102 | 106 | 113 | 109 | 125 | 130 | 129 | 127 | 128 | 54.3 - 57.2 | 75 ms
| AKAZE | 166 | 157 | 161 | 155 | 163 | 164 | 173 | 175 | 177 | 179 | 7.5 – 7.88 | 540 ms
| SIFT | 138 | 132 | 124 | 137 | 134 | 140 | 137 | 148 | 159 | 137 | 4.6 – 5.6 | 820 ms
 
<br/>
* We can notice from the table above that the BRISK detector detects the largest number of keypoints almost double the number of keypoits of the nearest detector. <br/>
* Also we can notice that ORB and BRISK  detectors  produce the largest avg Neighborhood size. <br/>
* Also we can notice that Fast  is the fastet keypoint detector with avrage 12 ms for the 10 images. <br/>


### MP.8 Performance Evaluation 2 <br/>

* Different combinations of detectors/descriptors types have been tested using the BF approach with the descriptor distance ratio set to 0.8 with `NORM_HAMMING` matching except with SIFT descriptor we used the `NORM_L2`.<br/> 
*  Some combinations of detectors/ descriptors are not compatible which indicated as (`---`) in the table.<br/> 

* The following table summarizes the number of matched keypoints for the 10 images using different combinations of detector/ descriptors.<br/>

| Detector/Descriptor | BRISK | BRIEF | ORB | FREAK | AKAZE | SIFT |
| :---:    | :---:  | :---:  | :---:  |  :---: | :---:  | :---:  |
| SHI-TOMASI | 763	| 944	| 907	| 766 | --- | 927
| HARRIS | 142	| 173	| 160	| 146	| --- | 163
| FAST | 899 |	1099 | 1081 | 881 | --- | 1046
| BRISK | **1570**	| **1704** | 1509 | 1526 | --- | **1649**
| ORB | 751 | 545 | 761 | 421 | --- | 763
| AKAZE | 1215 | 1266 | 1180 | 1188 | 1259 |	1270
| SIFT | 592 | 702 | --- |	596 |	--- |	800


* The bold numbers indicate the  Detector/Descriptor  combinations that produce the greatest number of matched keypoints: <br/>

  * BRISK/BRIEF
  * BRISK/SIFT
  * BRISK/BRISK

  <br/>
* WE can notice also that some combinations of FAST and AKAZE detectors produce also good number of matched keypoint.  <br/>
<br/>



### MP.9 Performance Evaluation 3

<br/>
* The following table summarizes time it takes for keypoint detection and descriptor extraction in (ms) for the 10 images using different combinations of detectors/descriptors: <br/>

| Detector/Descriptor | BRISK | BRIEF | ORB | FREAK | AKAZE | SIFT |
| :---:    | :---:  | :---:  | :---:  |  :---: | :---:  | :---:  |
| SHI-TOMASI |118.6|	99.94|	110.62|	451.68| --- | 208.04
| HARRIS | 109.37 | 124.15	|128.15|	462.5|	---	|243.1
| FAST | **31.55**|	**22.7**|	**26.06**	|374.6|	---|	161.52
| BRISK | 348.91|	338.59|	376.56|	689.78|	---	|532.26
| ORB | 85.51|	81.74	|127.02	|431.48|	---|	309.02
| AKAZE | 561.64	|545.14|	573.57|	878.41|	988.25|	713.59
| SIFT |723.15	|842.39|	---	|1236.09|	---	|1271.48

* The bold numbers indicate the  Detector/Descriptor  combinations that produce the lowest runing time : <br/>

* These 3 combinations of detector and descriptor are very fast to run and also produce quite sufficient number of keypoints matched. <br/>

  * FAST/BRIEF 
  * FAST/ORB
  * FAST/BRISK
  <br/>

* If we want to use another detector instead of the FAST detector I'll recommend. <br/>

  * ORB/BRISK
  * ORB/BRIEF
 










