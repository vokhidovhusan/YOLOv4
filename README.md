# YOLOv4 on Windows 10
### Requirements

### 1. Clone the darknet repository
```
git clone https://github.com/AlexeyAB/darknet.git
```
### 2. Compile on Windows (using vcpkg)
1. Install Visual Studio 2017 or 2019 english version. 
2. Install CUDA enabling VS Integration during installation
3. Open Powershell (Start -> All Programms -> Windows) and type following command:
```bash
cd darknet
.\build.ps1 -UseVCPKG -EnableOPENCV -EnableCUDA -EnableCUDNN -EnableOPENCV_CUDA
```
(you can remove options like -EnableOPENCV_CUDA, -EnableCUDA, -EnableCUDNN if you are not interested in them)