# YoloV3Detector

## Overview

This project is to detect the license plate of cars using Yolov3 model and recognize the plate number using OCR. 

The training dataset for Yolov3 model with the detected license plate is made manually from the images and video frames.
The recognition of number can not be solved by OCR using the tesseract framework. 

## Configuration

- darknet
    
    The source code to train the Yolov3 model with the train dataset.
        
- training_dataset
    
    Annotated dataset with images and xmls
    
- utils
    
    Several functions to prepare for training dataset

## Installation

- Environment

    Ubuntu 18.04, Python 3.6, GPU
    
- Dependency Installation

    ```
        pip3 install -r requirements.txt
    ```

## Execution

- Please run the following command in terminal in this project directory.

    ```
        python3 main.py
    ```

## Note

If you want to train the Yolov3 model and the handwritten recognition model, please read the following instructions.

- Training Yolo model
    
    * Preparation for training Yolo model
    
        Please download the darknet framework from https://github.com/pjreddie/darknet, make custom_data directory in it, 
        make the files whose name are custom.names and detector.data referencing 
        https://blog.francium.tech/custom-object-training-and-detection-with-yolov3-darknet-and-opencv-41542f2ff44e.
        Also, in the custom_data directory, the test and train text file is needed. The process to make these files is 
        referred in the execution step. And insert labels directory and images directory which contains the xml and jpg
        files for training in the custom_data folder. 
        
        Then download yolov3.weights from https://pjreddie.com/darknet/yolo/ and 
        darkenet53.conv.74 from https://pjreddie.com/media/files/darknet53.conv.74 and copy them in darknet directory.
    
    * Train the model to detect license plate
        
        After preparing for training as mentioned above, configure yolov3.cfg file followed by Configurations part in 
        https://blog.francium.tech/custom-object-training-and-detection-with-yolov3-darknet-and-opencv-41542f2ff44e
        If using GPU in training, please follow the instructions:
        
            1. Edit Makefile GPU=1
            2. Edit Makefile CUDNN=1
            3. Leave Makefile OPENCV=0
            4. Install re-install CUDA using pip (my advice is to use version 10.1)
            5. Edit Makefile NVCC=/usr/local/cuda/bin/nvcc (failure to do this will result in a recipe for target 'obj/convolutional_kernels.o' error during compilation)
            6. Compile (or recompile if you previously compiled) the Darknet binary build via admin privileges -- SUDO
            
            Also, modify batch size of yolov3-custom.config in custom_data/cfg according to image size.
            Following reference site, we may set batch size 64, but in our case, there happened an out of memory issue 
            while training.
            After modifying it 32, we could perform train well.
            
        Then plesae run the following command in the terminal. 
        ```
            ./darknet detector train custom_data/detector.data custom_data/cfg/yolov3-custom.cfg darknet53.conv.74
        ```
        
        After finishing train, copy the trained model(.weights) to utils/model/models/lp_detection_model directory.
      
## Main reference site

    https://blog.francium.tech/custom-object-training-and-detection-with-yolov3-darknet-and-opencv-41542f2ff44e
