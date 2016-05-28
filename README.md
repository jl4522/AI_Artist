# Neural Style Transfer
Implementation of Neural Style Transfer from the paper A Neural Algorithm of Artistic Style(http://arxiv.org/abs/1508.06576) in Keras 1.0.2

Uses the VGG-16 model as described in the Keras example below :
https://github.com/fchollet/keras/blob/master/examples/neural_style_transfer.py

## Weights (VGG 16)

Before running this script, download the weights for the VGG16 model at:
https://drive.google.com/file/d/0Bz7KyqmuGsilT0J5dmRCM0ROVHc/view?usp=sharing
(source: https://gist.github.com/baraldilorenzo/07d7802847aaad0a35d3)
and make sure the variable `weights_path` in this script matches the location of the file.

- Save the weights in the root folder
- Save a copy of the weights in the root of the windows_helper folder (if you wish to use the Windows Helper tool to easily execute the script)

## Modifications to original implementation :
- Uses 'conv5_2' output to measure content loss.
Original paper utilizes 'conv4_2' output

- Initial image used for image is the base image (instead of random noise image)
This method tends to create better output images, however parameters have to be well tuned.
Therefore their is a argument 'init_image' which can take the options 'content' or 'noise'


- Uses AveragePooling2D inplace of MaxPooling2D layers
The original paper uses AveragePooling for better results

- Style weight scaling
- Rescaling of image to original dimensions, using lossy upscaling present in scipy.imresize()
- Maintain aspect ratio of intermediate and final stage images, using lossy upscaling

Note : Aspect Ratio is maintained only if image is not rescaled.
       If image is rescaled to original dimensions then aspect ratio is maintained as well.

## Windows Helper
It is a C# program written to more easily generate the arguments for the python script Network.py

- Make sure to save the vgg16_weights.h5 weights file in the windows_helper folder.
- Upon first run, it will request the python path. Traverse your directory to locate the python.exe of your choice (Anaconda is tested)

### Benefits 
- Automatically executes the script based on the arguments.
- Easy selection of images (Content, Style, Output Prefix)
- Easy parameter selection
- Easily generate argument list, if command line execution is preferred. 

# Examples
<img src="https://raw.githubusercontent.com/titu1994/Neural_Style_Transfer/master/images/inputs/content/blue-moon-lake.jpg" width=400 height=300> <img src="https://raw.githubusercontent.com/titu1994/Neural_Style_Transfer/master/images/inputs/style/starry_night.jpg" width=400 height=300>
<br> Result after 50 iterations <br>
<img src="https://raw.githubusercontent.com/titu1994/Neural_Style_Transfer/master/images/output/Blue_Moon_Lake_iteration_50.png" width=600 height=300>
<br> DeepArt.io result <br>
<img src="https://raw.githubusercontent.com/titu1994/Neural_Style_Transfer/master/images/output/DeepArt_Blue_Moon_Lake.png" width=600 height=300>

# Network.py in action
![Alt Text](https://raw.githubusercontent.com/titu1994/Neural-Style-Transfer/master/images/Blue%20Moon%20Lake.gif)

# Requirements 
- Theano
- Keras 
- CUDA (GPU)
- CUDNN (GPU)
- Scipy
- Numpy

# Speed
On a 980M GPU, the time required for each epoch depends on mainly image size (gram matrix size) :

For a 400x400 gram matrix, each epoch takes approximately 11-13 seconds. <br>
For a 512x512 gram matrix, each epoch takes approximately 18-22 seconds. <br>
For a 600x600 gram matrix, each epoch takes approximately 28-30 seconds. <br>
  
# Issues
- Due to usage of content image as initial image, output depends heavily on parameter tuning. <br> Test to see if the image is appropriate in the first 10 epochs, and if it is correct, increase the number of iterations to smoothen and improve the quality of the output.
- Generated image is seen to be visually better if a small image size is used.
- Due to small gram sizes, the output image is usually small. 
<br> To correct this, use the implementations of this paper "Image Super-Resolution Using Deep Convolutional Networks" http://arxiv.org/abs/1501.00092 to upscale the images with minimal loss.
<br> <br> <br> Some implementations of the above paper for Windows : https://github.com/tanakamura/waifu2x-converter-cpp

Overview
============
A project that trains a  convolutional neural network over a dataset to repaint an novel image in the style of a given painting. This is the implementation of Neural Style Transfer from the paper A Neural Algorithm of Artistic Style(http://arxiv.org/abs/1508.06576) in Keras 1.0.2. This is also the code for 'Build an AI Artist' on [Youtube](https://youtu.be/S_f2qV2_U00)

![Alt Text](https://raw.githubusercontent.com/titu1994/Neural-Style-Transfer/master/images/Blue%20Moon%20Lake.gif)


Dependencies
============

* Numpy (http://www.numpy.org/)
* Keras (http://keras.io/#installation)
* Scipy  (https://www.scipy.org/install.html)
* Theano (http://deeplearning.net/software/theano/install.html#install) 
* h5py (http://docs.h5py.org/en/latest/build.html)
* sklearn (http://scikit-learn.org/stable/install.html)
* CUDA (GPU) (http://docs.nvidia.com/cuda/cuda-getting-started-guide-for-mac-os-x/)
* CUDNN (GPU) (https://developer.nvidia.com/cudnn)

Use [pip](https://pypi.python.org/pypi/pip) to install any missing dependencies

If you have dependency version issues, use [virtualenv](http://docs.python-guide.org/en/latest/dev/virtualenvs/) 

Basic Usage
===========

There are 3 images to identify when we run the script

1. Your base image (to artify)
2. Your reference image (the art to learn from)
3. Your generated image

Run `python network.py --base_image_path /path/to/your/image Shot --style_reference_image_path /path/to/your/painting --result_prefix /path/to/generated/file/you/create` to generate an image in your chosen style

Other optional commands are 

`--image_size`: Size of your output image
`--content_weight`: How much to weigh the content
`--style_weight`: How much to weigh the style
`-style_scale`: How much to scale the style
`--total_variation_weight`: Uniformity of the generated image
`--num_iter`: Nmber of iterations
`--rescale_image`: to rescale or not to rescale
`--rescale_method`: rescale algorithm 
`--maintain_aspect_ratio`: to maintain aspect ratio or not 
`--content_layer`: which layer to focus on for content generation

On a 980M GPU, the time required for each epoch depends on mainly image size (gram matrix size) :

For a 400x400 gram matrix, each epoch takes approximately 11-13 seconds. 
For a 512x512 gram matrix, each epoch takes approximately 18-22 seconds. 
For a 600x600 gram matrix, each epoch takes approximately 28-30 seconds. 

Credits
===========
Credit for the vast majority of code here goes to [Yoav Zimmerman](https://github.com/yoavz). I've merely created a wrapper around all of the important functions to get people started.
