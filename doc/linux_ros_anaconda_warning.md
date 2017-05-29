### A Warning to Linux users who already have ROS installed:

Upon completion of the Anaconda install, you should have a new Anaconda environment called `RoboND` with all the packages you need.  However, if you have ROS installed on your Linux machine you will most likely run into a conflict between Python installations due to the `PYTHONPATH` variable being set in your `/opt/ros/kinetic/setup.bash` file which gets sourced in your `.bashrc` file.  

The hallmark of this conflict is getting an import error when you go to import packages that you just installed using Anaconda.  First you fire up Python at the command line and everything looks normal:

```sh
$ python
Python 3.5.2 |Anaconda custom (x86_64)| (default, Jul  2 2016, 17:52:12) 
[GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
Next you go to import a package (OpenCV or `cv2` in this case) and you get an `ImportError` like this (or similar):

```
>>> import cv2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: /opt/ros/kinetic/lib/python2.7/dist-packages/cv2.so: undefined symbol: PyCObject_Type
```

The problem here is that Python is looking for a package in `/opt/ros/kinetic/lib/python2.7/dist-packages/` when it should be looking in the Anaconda installation directories.  The reason for this is that with your ROS install, your `.bashrc` file contains the line `source /opt/ros/kinetic/setup.bash`, which sets your `PYTHONPATH` variable to the `/opt/ros/kinetic/...` location.  

The solution is to unset your `PYTHONPATH` variable before sourcing your Anaconda environment:

```
$ unset PYTHONPATH
$ source activate RoboND
```
And it should work!  But you need to remember to do this each time you want to use your Anaconda Python install.  

An alternative solution is to remove the line `source /opt/ros/kinetic/setup.bash` from your `.bashrc` file, but that means you'll need to manually execute that command each time you want to use ROS.  See [this post](https://stackoverflow.com/questions/43019951/after-install-ros-kinetic-cannot-import-opencv) and [this post](https://stackoverflow.com/questions/17386880/does-anaconda-create-a-separate-pythonpath-variable-for-each-new-environment) for more information on the matter.  