## Create the environment 

Using a linux terminal, navigate to the proyect folder and type:

> conda create -y --prefix ./env python=3.7

Then type:
>source activate ./env

## Install dependencies

Execute the following commands in order:

> conda install -y -c conda-forge opencv

> conda install -y -c anaconda cudatoolkit==10.1

> conda install -y cudnn==7.6.4

> conda install -y numpy

> conda install -y -c menpo imageio 

## Now you can compile the program, for this you have to first set the PKG_CONFIG_PATH variable to include the lib/pkgconfig folder from your environment:

> PKG_CONFIG_PATH=$PKG_CONFIG_PATH:./env/lib/pkgconfig

> export PKG_CONFIG_PATH

And compile
> make

## Download yolo weights

>wget https://pjreddie.com/media/files/yolo.weights

>wget https://github.com/leetenki/YOLOtiny_v2_chainer/blob/master/tiny-yolo-voc.weights

Rename this last file to tini-yolo.weights

## Prepare the video

Copy input video to video folder
then navigate with the console to the video folder (cd video from the previous directory)

change al print function in video2img from print 'something' to print('something')
execute the following command, replacing videoname.videotype for your video name and extension

> python video/video2img.py -i video/videoname.videotype

Now execute:
> python video/get_pkllist.py

## Change the main files:

### in yolo_seqnms.py change:
* all print functions to use parenthesis (as in prepare the video section)
* change import cpickle as pickle to import cpickle
* add import imageio
* in line 288 (maybe 289 or 300 with the previous changes) change scipy.misc.imsave to imageio.imwrite

### in yolo_detection.py change:
* all print functions to use parenthesis
* in line 139 change the strings to start with a b, like "yolo.weights" now should be b"yolo.weights".
* in line 113 change the image to image.econde('utf-8')

## Install some final dependencies:
> conda install -y matplotlib

> conda install -y -c anaconda scipy

> conda install -y -c conda-forge tensorflow

> pip install tensorflow-object-detection-api

## copy libdarknet.so and libdarknet.a to env/lib
> cp libdarknet.so env/lib/

> cp libdarknet.a env/lib/

## Run the code

> python yolo_seqnms.py
