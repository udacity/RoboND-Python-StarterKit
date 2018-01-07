### A Warning to Linux users who already have ROS installed:

Upon completion of the Anaconda install, you should have a new Anaconda environment called `RoboND` with all the packages you need.  However, if you have ROS installed on your Linux machine you will most likely run into a conflict between Python installations due to the `PYTHONPATH` variable being set in your `/opt/ros/kinetic/setup.bash` file which gets sourced in your `.bashrc` file.  

The hallmark of this conflict is getting an import error when you go to import packages that you just installed using Anaconda.  To test whether you have this problem, first you fire up Python at the command line:

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
And it should work!  But in this case, you'll need to remember to do this each time you want to use your Anaconda Python install.  

## Recommended ROS Approach

For a more permanent solution:

1. Install anaconda and choose not to add it to your path
2. Create a script named: ~/bin/cenv, with the following contents:
	```
	#!/bin/bash
	unset PYTHONPATH
	export PATH=$HOME/anaconda3/bin:$PATH
	source activate $1
	```
3. Login out and log back in to trigger the ~/bin path to be automatically added to your PATH variable.
	Then you can run:
	```
	. cenv RoboND
	```
	anywhere on your system and it just works.

### Example

**Note "." is the same as "source" and is easier to type**

```sh
$ . /opt/ros/kinetic/setup.bash 
$ which python
/usr/bin/python
$ echo $PYTHONPATH
/opt/ros/kinetic/lib/python2.7/dist-packages
$ . cenv RoboND
(RoboND) $ which python
~/anaconda3/envs/RoboND/bin/python
(RoboND) $ echo $PYTHONPATH

(RoboND) $ ipython
Python 3.5.2 | packaged by conda-forge | (default, Jan 19 2017, 15:28:33) 
Type 'copyright', 'credits' or 'license' for more information
IPython 6.0.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: import cv2
```

See [this post](https://stackoverflow.com/questions/43019951/after-install-ros-kinetic-cannot-import-opencv) and [this post](https://stackoverflow.com/questions/17386880/does-anaconda-create-a-separate-pythonpath-variable-for-each-new-environment) for more information on the matter.  
