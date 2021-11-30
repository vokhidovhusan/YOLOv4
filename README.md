# YOLOv4 on Windows 10

This tutorial is about how compile yolov4 (using vcpkg) and training on Windows

### Requirements
coming soon

### 1. Prepare project folder
1. Create project repositories
```bash
mkdir yolov4
cd yolov4
mkdir backup data model
```

2. Clone the darknet repository
Clone darknet repository in yolov4 project folder
```bash
git clone https://github.com/AlexeyAB/darknet.git
```

3. Compile on Windows (using vcpkg)

 -  Install Visual Studio 2017 or 2019 english version. 
 -  Install CUDA enabling VS Integration during installation
 -  Open Powershell (Start -> All Programms -> Windows) and type following command:

```bash
cd darknet
.\build.ps1 -UseVCPKG -EnableOPENCV -EnableCUDA -EnableCUDNN -EnableOPENCV_CUDA
```
(you can remove options like -EnableOPENCV_CUDA, -EnableCUDA, -EnableCUDNN if you are not interested in them)

### 3. Test Yolov4 on GPU
1. Download pre-trained weight file
You will have to download the pre-trained weight file and copy it into model folder. 

From google-drive you can download: https://drive.google.com/open?id=1cewMfusmPjYWbrnuJRuKhPMwRe_b9PaT                                    

OR

Just run this (Go to model folder and download by wget):
```bash
cd yolov4/model
wget https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.weights
```


2. Run the detector
For still image:
```
cd darknet
darknet.exe detector test cfg/coco.data cfg/yolov4.cfg ../model/yolov4.weights -ext_output data/dog.jpg
```

For video file (You need to find .avi/.mp4 video file yourself. Put the file in data folder):
```bash
cd darknet
darknet.exe detector demo cfg/coco.data cfg/yolov4.cfg ../model/yolov4.weights ../data/demo.mp4 -ext_output
```

### 4. Prepare your dataset

1. Data preparation
copy your dataset into data folder

### 5. Prepare necessary files for training 

1. Create yolov4.data file and fill it as following:
```
classes= 80 # class number 
train  = <path-to-dataset>/train.txt
valid  = <path-to-dataset>/val.txt
names = data/yolov4.names
backup = backup
```

2. Modify cfg file
 - copy yolov4-csp.cfg file from `darknet/cfg` to your `model` folder
 - open yolov4-csp.cfg and modify it. 
```
batch=64 # if CUDA Error: out of memory then make it lower
subdivisions=16 # if CUDA Error: out of memory try to modify it
width=512 # image size
height=512 # image size
...
classes=80 # class number 
filters=255 # You should find all three filters=255 and change according to your class number (class number + 5) * 3 
```

3. Create `yolov4.names` file in model folder and fill it with class names
```
aeroplane 
bicycle
...
...
train
tvmonitor
```

### 6. Train the Model

1. Download pre-trained weights for training
    https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v4_pre/yolov4-csp.conv.142

2. Train on 1 GPU for like 1000 iterations: 
```bash
cd darknet
darknet.exe detector train ../model/yolov4.data ../model/yolov4-csp.cfg ../model/yolov4.conv.142
```

4. Then stop and by using partially-trained model ../backup/yolov4_1000.weights run training with multigpu (up to 4 GPUs): 

```bash
cd darknet
darknet.exe detector train ../model/yolov4.data ../model/yolov4-csp.cfg ../backup/yolov4_1000.weights -gpus 0,1,2,3
```

