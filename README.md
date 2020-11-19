# Seq_nms_YOLO

Based on the work from https://github.com/melodiepupu/seq_nms_yolo
Adapted to work on python 3.7 with cuda 10.1

This project combines **YOLOv2**([reference](https://arxiv.org/abs/1506.02640)) and **seq-nms**([reference](https://arxiv.org/abs/1602.08465)) to realise **real time video detection**.

## How to use:

### Create the conda environment 

Using a linux terminal, navigate to the proyect folder and type:

> conda create -y --prefix ./env python=3.7

Then type:
>source activate ./env

### Install dependencies

Execute the following commands in order:

> conda install -y -c conda-forge opencv

> conda install -y -c anaconda cudatoolkit=10.1

> conda install -y cudnn=7.6.4

> conda install -y numpy

> conda install -y -c menpo imageio 

### Now you can compile the program, for this you have to first set the PKG_CONFIG_PATH variable to include the lib/pkgconfig folder from your environment:

> PKG_CONFIG_PATH=$PKG_CONFIG_PATH:./env/lib/pkgconfig

> export PKG_CONFIG_PATH

### Compile the program

> make

### Download yolo weights
Execute the following commands to download the weights. First one will take a while.

> wget https://pjreddie.com/media/files/yolo.weights

> wget https://github.com/leetenki/YOLOtiny_v2_chainer/blob/master/tiny-yolo-voc.weights

Rename this last file from tiny-yolo-voc.weights to tini-yolo.weights

> mv tiny-yolo-voc.weights tiny-yolo.weights

### Prepare the video

Copy input video to video folder
then navigate with the console to the video folder (cd video from the previous directory)
> cd video

execute the following command, replacing videoname.videotype for yourpor video name and extension

> python video2img.py -i video/videoname.videotype

Now execute:

> python get_pkllist.py

### Install some final dependencies:
> conda install -y matplotlib

> conda install -y -c anaconda scipy

> conda install -y -c conda-forge tensorflow

> pip install tensorflow-object-detection-api

### copy libdarknet.so and libdarknet.a to env/lib
First go back to the main directory 
> cd..
Now execute
> cp libdarknet.so env/lib/

> cp libdarknet.a env/lib/


### Run the code
If you want to run it with seq-nms:
> python yolo_seqnms.py
if you want to run it without seq-nms:
> python yolo_no_seqnms.py

### Extract the video
Type
> cd video

And enerate the video with:
> python img2video.py -i output

## References

This project copies lots of code from [darknet](https://github.com/pjreddie/darknet) , [Seq-NMS](https://github.com/lrghust/Seq-NMS) and  [models](https://github.com/tensorflow/models).
