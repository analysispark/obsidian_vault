# How to Safely Install OpenCV on the Mac M1  
##   
##   
## A Preamble on OpenCV and the M1  
OpenCV is one of the most popular computer vision libraries. It provides a large set of image manipulation tools that are very useful for processing images and image streams often for use with machine learning algorithms. OpenCV is a large library with a lot of dependencies though. Its dependency list includes packages like ffmpeg, libgfortran, and pixman which is where the issues start, especially for m1. Macs with M1 processors use the arm64 architecture which is incompatible with the amd64 architecture used by the likes of Intel and AMD. However, computers using arm64 processors are a very small subset compared to the billions of amd64 computers that have been around for the last 15 years. This means that most software is still built to run on amd64 processors so when we install OpenCV, there is a strong chance your computer, or dependency manager, tries to install an amd64 version. On M1 this is a problem and instead we want to the arm64 versions of OpenCV and its dependencies.  
To solve this inconsistency, we use virtual environments in miniforge. Virtual environments are a great way to separate different dependencies if you need different versions for different projects or if you don’t want to change your system path and its dependencies. One of the most popular packages to accomplish this is Anaconda, but anaconda is bloated and doesn’t always have the right packages for m1. Instead, we use miniforge, a community package that provides a minimalistic Anaconda install and uses the conda-forge branch to install packages. Conda-forge provides community packages for Anaconda which has a much larger library of arm64 compatible packages.  
##   
## Installation of OpenCV on the M1  
The first step is to install Homebrew if you don’t have it. Homebrew is a dependency manager for MacOS, and we will use it to install miniforge.  
Installing Homebrew  
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"  
  
Then we install miniforge using Homebrew.  
brew install miniforge  
conda init zsh  
1  
2  
Next we can set up a new virtual environment called cv with python 3.8.6.  
conda create -n cv python=3.8.6  
conda activate cv  
1  
2  
Lastly, we can install OpenCV  
conda install -c conda-forge opencv  
  
Now every time you want to use opencv you can use the “cv” conda environment by using with the command conda activate cv. To keep further dependencies in this environment, just install them after you activate the environment using the conda install command.  
## Why Can't I install OpenCV on the M1 with PIP?  
Even after you read the above, you might still want to install OpenCV on your M1 with PIP. Be advised - pip has a package that can work. The problem is that pip does not always install the arm64 version and instead tries to use rosetta to run amd64 versions which breaks. So it “works” it is just finicky and error prone.  
miniforge  specifically supports arm64 and will always grab arm64 versions that have been precompiled to run on m1.  
##   
## Common Errors  
Library not loaded: /opt/homebrew/opt/ffmpeg/lib/libavcodec.58.dylib  
This is caused by the incompatible, amd64, version of ffmpeg being installed and then trying to be loaded by OpenCV. This is why the conda-forge opencv package is recommended.  
