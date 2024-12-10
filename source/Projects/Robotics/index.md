# Robotics

I am on the [FRC#2022 Titan Robotics](https://titanrobotics2022.com/) team, which competes in the FIRST Robotics Competition every year. Our robot has two main processors -- the roboRIO, a Linux-RT (realtime) system with an FPGA which does real-time I/O on the CAN bus in order to control the robot's motors and other actuators and recieves input from sensors such as encoders and beambreakers, and the coprocessor (this year, we switched from Raspberry Pi and Jetson Nano to the RK3588-based Orange Pi 5 Plus), which takes in input from the cameras to localize the robot using AprilTags and performs object detection in the seasons that we need it. 

## Robot control

<https://github.com/titan2022/FRC-2024-JAVA>

I worked on the robot control code for the 2024 season, in many small areas during the season.

During competition I helped fix small bugs that were causing issues during the matches.

In the postseason, I did a lot of work to fix other bugs we were encountering and migrate code to newer dependencies that had a lot of breaking changes.

## Vision

<https://github.com/titan2022/FRC-2024-Vision>

I worked with another sophomore to build a YOLOv8 model to detect the game piece, and was working on using the Intel RealSense camera to determine the game piece's 3D-space location relative to the bot. I also made C and Python bindings for our C++ networking library, which used our custom UDP protocol (the standard protocol in FRC), so that we could send information to the roboRIO. However, our intake subsystem was large enough that the driver did not need precise alignment, so the object detection was unnecessary.

## Coprocessor

I worked on a lot of things related to the coprocessors. I packaged OpenCV for the coprocessors, which was very error-prone as the configuration and dependencies needed to be precisely defined so that we would get good GPU-accelerated performance on our antiquated operating system. I set up deployment and autostarting scripts in order to make it easy to get the robot into match-ready state. I also worked on coprocessor selection and operating system selection (which can be difficult for embedded devices due to poor vendor support and the large variety of third-party OSes).

## Packaging

### Debian packages

<https://github.com/ethanc8/titanian-repo>

I made Debian packages for the software we use often, including on our development laptops and on the ARM coprocessors. This required a lot of coprocessor-specific handling, as CUDA support, OpenCL support, and various other things vary between them. Also, the coprocessors are not very well supported in general, so I had to work around that. In particular, the last supported OS on Jetson Nano is Ubuntu 18.04, so I had to backport a bunch of different packages in order to get our software to build. Additionally, in order to get librealsense2 to work on non-Ubuntu kernels, I had to patch the DKMS modules.

This includes builds of OpenCV, vcpkg, librealsense2, and their dependencies.

I eventually realized that Debian packages were hard to test and use on non-Debian systems which some of our members used, and that they didn't provide many advantages over Conda packages.

### Conda packages

<https://github.com/ethanc8/titan-forge-feedstocks>

I am switching everything over to Conda now, which not only solves everything that APT did, but it allows us to create environments with different package versions and handles dependencies like `nlohmann/json` that we used to use `vcpkg` for. I build the packages using [`rattler-build`](https://rattler.build/latest/), and I set up CI/CD on GitHub Actions in order to automatically build and publish packages when the recipes are changed.

## Fall 2024 training lessons

<https://titanrobotics2022.com/docs/dev/PreseasonTraining/index.html>

I ran some of the fall 2024 programming-team training lessons, including [the 2024 code overview](https://titanrobotics2022.com/docs/dev/PreseasonTraining/2024CodeOverview/index.html). I also wrote many of the writeups for earlier topics, and helped [@nekiwo](https://github.com/nekiwo) with the lessons he taught. These lessons were quite successful, as we got almost entirely positive feedback and were able to keep our students engaged much longer than in past seasons. I also helped solve technical issues that our students faced, including software installation and Linux setup. Linux setup was particularly difficult as many users needed newer kernels (including kernels that had not yet been packaged for Debian backports), had buggy BIOSes, or had badly configured Windows installations that needed to be recovered after installing Linux. 
