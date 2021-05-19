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

## Rubric points addressed
1. MP.1 - A ring buffer of size 2 has been implemented using standard library Queue
2. MP.2 - The various detectors are implemented which can be chosen by the string detectorType
3. MP.3 - All the keypoints outside the pre-defined rectangle have been removed using OpenCV's Rect.contains function
4. MP.4 - The various descriptors are implemented which can be chosen by the string descriptorType
5. MP.5 - FLANN and KNN matching have been implemented
6. MP.6 - Descriptor distance ratio has been implemented
7. MP.7 - 
### Descending order of average detected keypoints on the preceding vehicle 
||||||||
|-------|-------|-------|-------|-----------|------|--------|
| BRISK | AKAZE | SIFT  | ORB   | SHITOMASI | FAST | HARRIS |
| 276.2 | 167   | 138.6 | 116.1 | 117.9     | 94.9 | 24.8   |

### Remarks on detectors
* Brisk - Densly clouded points, mainly at top half of the vehicle, consistent

* AKAZE - Keypoints are spread out along the contour of the preceding vehicle and are consisten

* SIFT  - Taillights are recognised consistently but overall the keypoints do not seem very consisten

* ORB   - Consistent but sparse and big neighbourhoods considered for every keypoint

* SHITOMASI   - Spread out, consistent and small neighbourhoods considered for every keypoint

* FAST   - Keypoints are sparse, spread along the contour of the preceding vehicle and the neighbourhoods are approx 10x10 pixels

* HARRIS   - Very sparse keypoints (could be because of corner response threshold) but are consitent and neighborhoods similiar to FAST detector

8. MP.8 - 
### Good Features to Track (Shitomasi)
| BRISK | BRIEF | ORB | FREAK | AKAZE | SIFT |
|-------|-------|-----|-------|-------|------|
| 118   | 118   | 118 | 118   | -     | 118  |
### HARRIS
| BRISK | BRIEF | ORB | FREAK | AKAZE | SIFT |
|-------|-------|-----|-------|-------|------|
| 23    | 23    | 23  | 23    | -     | 23   |
### FAST
| BRISK | BRIEF | ORB | FREAK | AKAZE | SIFT |
|-------|-------|-----|-------|-------|------|
| 95    | 95    | 95  | 95    | -     | 95   |
### BRISK
| BRISK | BRIEF | ORB | FREAK | AKAZE | SIFT |
|-------|-------|-----|-------|-------|------|
| 278   | 278   | 278 | 278   | -     | 278  |
### ORB
| BRISK | BRIEF | ORB | FREAK | AKAZE | SIFT |
|-------|-------|-----|-------|-------|------|
| 105   | 114   | 114 | 61    | -     | 114  |
### AKAZE
| BRISK | BRIEF | ORB | FREAK | AKAZE | SIFT |
|-------|-------|-----|-------|-------|------|
| 165   | 165   | 165 | 165   | 165   | 165  |
### SIFT
| BRISK | BRIEF | ORB | FREAK | AKAZE | SIFT |
|-------|-------|-----|-------|-------|------|
| 138   | 138   | -   | 137   | -     | 138  |
9. MP.9 - 
### Descriptors arranged based on fastness (in ms)
| BRIEF             | BRISK             | ORB               | SIFT              | FREAK             | AKAZE      |
|-------------------|-------------------|-------------------|-------------------|-------------------|------------|
| 0.773459757143    | 2.112561857143    | 7.854222833333    | 28.368754285714   | 31.722652857143   | 46.60965   |

---

## Suggested Top-3 detector-descriptor combinations for real time applications are
1. FAST + BRIEF or BRISK
2. ORB + BRIEF or BRISK
3. SHITOMASI + BRIEF or BRISK


