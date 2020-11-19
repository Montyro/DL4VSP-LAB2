## Create the environment 

Using a linux terminal, navigate to the proyect folder and type:

> conda create -y --prefix ./env python=3.7

Then type:
>source activate ./env

## Install dependencies

Execute the following commands in order:

> conda install -y -c conda-forge opencv

> conda install -y -c anaconda cudatoolkit=10.1

> conda install -y cudnn=7.6.4

> conda install -y numpy

> conda install -y -c menpo imageio 

## Now you can compile the program, for this you have to first set the PKG_CONFIG_PATH variable to include the lib/pkgconfig folder from your environment:

> PKG_CONFIG_PATH=$PKG_CONFIG_PATH:./env/lib/pkgconfig

> export PKG_CONFIG_PATH

## In makefile edit:
To the ARCH = -gencode .... add at the end of the last line:
> \ 
and in the next line:
> -gencode arch=compute_75,code=[sm_75,compute_75]

In the line 50 (or 49 if you did not add previous line) change cuda-8.0 to cuda-10.1
and two lines below, also change cuda-8.0 to cuda-10.1

And compile
> make

## Download yolo weights
Execute the following commands to download the weights. First one will take a while.

> wget https://pjreddie.com/media/files/yolo.weights

> wget https://github.com/leetenki/YOLOtiny_v2_chainer/blob/master/tiny-yolo-voc.weights

Rename this last file from tiny-yolo-voc.weights to tini-yolo.weights
> mv tiny-yolo-voc.weights tiny-yolo.weights


## Prepare the video

Copy input video to video folder
then navigate with the console to the video folder (cd video from the previous directory)
> cd video

change al print function in video2img from print 'something' to print('something') (2 in total)
execute the following command, replacing videoname.videotype for yourpor video name and extension

> python video2img.py -i video/videoname.videotype

Now execute:
> python get_pkllist.py

## Change the main files:

### in yolo_seqnms.py change:
* all print functions to use parenthesis (as in prepare the video section) (8 in total)
* change import cpickle as pickle to import pickle
* add import imageio
* in line 288 (maybe 289 or 300 with the previous changes) change scipy.misc.imsave to imageio.imwrite
* in line 39 (40 with the previous changes), change  box[0] == cls to str(box[0],'utf-8') == cls
### in yolo_detection.py change:
* all print functions to use parenthesis (5 in total)
* in line 138 change all the strings to start with a b, like "yolo.weights" now should be b"yolo.weights".
* in line 113 change the image to image.enconde('utf-8')

## Install some final dependencies:
> conda install -y matplotlib

> conda install -y -c anaconda scipy

> conda install -y -c conda-forge tensorflow

> pip install tensorflow-object-detection-api

## copy libdarknet.so and libdarknet.a to env/lib
First go back to the main directory 
> cd..
Now execute
> cp libdarknet.so env/lib/

> cp libdarknet.a env/lib/

## Run the code

> python yolo_seqnms.py

## Export the video
> cd video

change al print function in img2video from print 'something' to print('something') (2 in total)

Generate the video:
> python img2video -i output
