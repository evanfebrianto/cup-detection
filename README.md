[Cuda Installation]: https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#installing-cuda-development-tools
[cuDNN Installation]: https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html#install-windows
[protoc]: https://github.com/google/protobuf/releases/download/v3.3.0/protoc-3.3.0-win32.zip
[labelImg]: https://github.com/tzutalin/labelImg

# Pre-requisites
1.  Install Git for Windows
2.  Install CUDA = 10.1, see the details here: [Cuda Installation]
3.  Install cuDNN = 7.6, see: [cuDNN Installation]
4.  Install Anaconda for Windows

# Installation
1. Create environment with python 3.8: 
`conda create tf2 = python=3.8`
2. Activate environment: `conda activate tf2`
3. Download libraries:
```pip install opencv-python pillow notebook tensorflow-gpu==2.3.*
pip install pillow lxml jupyter matplotlib cython pandas contextlib2
conda install -c conda-forge protobuf
pip install pycocotools tf_slim lvis tqdm
```

# Environment Configuration
1. Download [protoc] for windows and copy `protoc-3.3.0-win32` to `C:\` directory
2. Add `C:\protoc-3.3.0-win32\bin` to path system variable
3. Add PYTHONPATH variable in your system environment with `<your-path>\content\models` as the value

# How to Use
1. Open `pipeline.ipynb` and follow the steps
2. You may need to change `content\models\research\object_detection\model_main_tf2.py` on line 38 regarding your GPU memory. Change `[tf.config.experimental.VirtualDeviceConfiguration(memory_limit=2560)])` to be `[tf.config.experimental.VirtualDeviceConfiguration(memory_limit=1024)])` if you want to allocate 1GB of your GPU memory. Please consider that there might be other programs that utilize your GPU memory, so DO NOT allocate all your GPU memory here. The rule of thumb is you can allocate 75% of your GPU memory. If you have 2GB memory, then you can allocate 1.5GB for the process. 1GB = 1024MB and you can only put integer in this parameter. 
3. You also need to change `models\tf2\my_centernet_resnet50_v1_fpn\pipeline.config` to configure your training parameters such as batch_size, steps, checkpoint, etc.

# How to Train
1. Put all your images in `images/` folder
2. Annotate your image using [labelImg] in PASCAL VOC format
3. Change `annotations\label_map.pbtxt` accordingly to match your labels
4. Then follow the steps written in the `pipeline.ipynb`

# How to Test Using Webcam
1. Open `pipeline.ipynb`
2. Run `Set configurations` section
3. Run `Load the trained model` section
4. Go to `Run on realtime webcam` section and change `cap = cv2.VideoCapture(cv2.CAP_DSHOW + 1)` according to your camera device ID. If you are using an internal webcam, you can set to `cap = cv2.VideoCapture(cv2.CAP_DSHOW + 1)`. Otherwise, you do not need to change.