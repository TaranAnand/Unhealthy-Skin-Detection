# Unhealthy-Skin-Detection
A walkthrough of how I planned to do this project and the issues that prevented me from doing so.

First, I want to be clear that I tried dozens of possible solutions and spent hours trying to make this work. However, at one point, I realized that it just was not going to work, and I decided to make this document as a walkthrough of my attempt to finish the final project.

I started off the project by looking into how I wanted to approach the topic. To be clear, my attempt was to make a rough skeleton of a skin cancer detection program. With the given time and resources, I would definitely not be able to make something very sophisticated, but I planned for it to be capable enough to detect unhealthy skin at the least.

My first attempt with the project began in headless mode, and I tried to use a portion of the HAM10000 dataset for dermatoscopic images by importing some images into my Jetson Nano. This did not work out. Navigating the CVAT tool to make annotations and then attempted to move the entire dataset over to the Jetson Nano proved to be very time consuming and impractical.

I then tried to use the camera-capture function on the Jetson Nano to take images for my own dataset. However, after trying this in headless, I realized that the OpenGL software wouldn't work, and my attempts to use RTP to stream video output to my main pc proved fruitless. I moved over to headed mode following this endeavour.

For a while, this seemed to work. Eventually, I finished creating my dataset and was ready to move on to the next step. However, at this point, everything went south. 

I tried to use the instructions in the jetson-inference github to train a model using my dataset:
$ cd jetson-inference/python/training/classification
$ python3 train.py --model-dir=models/<YOUR-MODEL> data/<YOUR-DATASET>

However, I started off with an error saying torch was not defined in the train.py file. After installing and compiling Pytorch 1.6 for Python 3.6.9, I ran through the commands again and was met with another error. I got recurrent errors saying that there were no acceptable files in my dataset that were in the right format. Honestly, I do not recall how I fixed this issue, but, as I tried to train the model, I got an error that was impossible for me to fix with the limited time.

I kept getting an error that said SyntaxError: future feature annotations is not defined.

After hours of research trying to find a fix, the only fixes I saw involved update to Python 3.7. However, after updating to Python 3.7, I was met with the torch is not defined error again. I tried to use the jetson-inference instructions to update Pytorch, but it did not support Pytorch 1.7 for Python 3.7. I found out that I needed to build Pytorch from the source for Python 3.7, which I attempted to do, but ended up running out of memory/space on the Jetson Nano.

Another fix I attempted was to edit the _deprecate.py file to remove the from __future__ import annotations. A solution following these lines was the most common thing I found online. However, when I tried to make edits to the file, I was told that the file would not update in the archive. When I tried to edit the file outside the archive and put it back in, I was told that I had insufficient permissions to make changes in the folder.

Other than the errors I faced, the way I wanted to work through the project was to finish training my model using my dataset. After that, I would export it to ONNX to use in my python project to recognize unhealthy skin.
