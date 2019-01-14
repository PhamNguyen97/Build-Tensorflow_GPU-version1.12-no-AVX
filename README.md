# Build-Tensorflow_GPU-version1.12-no-AVX
Tensorflow_GPU version 1.12 for old CPU with no AVX support.

# Build from source
Build a TensorFlow pip package from source and install it on Ubuntu Linux and macOS.

## Install Software requirements : link _ https://www.tensorflow.org/install/gpu
The following NVIDIA® software must be installed on your system:

- NVIDIA® GPU drivers —CUDA 9.0 requires 384.x or higher. ( SHOULD BE VERSION 9.0)
- CUDA® Toolkit —TensorFlow supports CUDA 9.0.
- CUPTI ships with the CUDA Toolkit.
- cuDNN SDK (>= 7.2)
- (Optional) NCCL 2.2 for multiple GPU support.
- (Optional) TensorRT 4.0 to improve latency and throughput for inference on some models.

## Install Python and the TensorFlow package dependencies UBUNTU,MAC OS
sudo apt install python-dev python-pip  # or python3-dev python3-pip

## Install the TensorFlow pip package dependencies (if using a virtual environment, omit the --user argument):
pip install -U --user pip six numpy wheel mock
pip install -U --user keras_applications==1.0.6 --no-deps
pip install -U --user keras_preprocessing==1.0.5 --no-deps

## Install Bazel 0.19.0 _ For UBUNTU:
#### Step 1: Install required packages:
sudo apt-get install pkg-config zip g++ zlib1g-dev unzip python

#### Step 2: Download Bazel. (bazel-0.19.2-installer-linux-x86_64.sh):
https://github.com/bazelbuild/bazel/releases

#### Step 3: Run the installer:
chmod +x bazel-0.19.2-installer-linux-x86_64.sh
./bazel-0.19.2-installer-linux-x86_64.sh --user

#### Step 4: Set up your environment:
export PATH="$PATH:$HOME/bin"

#### Step 5: Install the JDK:
sudo apt-get install openjdk-8-jdk

#### Step 6: Add Bazel distribution URI as a package source:
echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -

#### Step 7: Install and update Bazel:
sudo apt-get update && sudo apt-get install bazel

# BUILD TENSORFLOW:
### Download the TensorFlow source code
git clone https://github.com/tensorflow/tensorflow.git
cd tensorflow
git checkout branch_name  # r1.9, r1.10, etc. (for example, git checkout r1.12 for tensorflow version 1.12)

### Configure the build
./configure
*** (I hoose No for all except for those say about CUDA)

### BUILD _ GPU support:
Using bazel version of bazel 0.19.0, Added the content of file "/home/<user>/tensorflow/tools/bazel.rc" on top of (hidden) file "/home/<user>/tensorflow/.tf_configure.bazelrc". Then run the followings: 
bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
./bazel-bin/tensorflow/tools/pip_package/build_pip_package /mnt  # create package

### Copy .whl file built in the previous step into place you want:
nautilus /mnt

# Install tensorflow: In the folder you put your .whl file:
pip3 install <name_of_file>.whl

for example: pip3 install tensorflow-1.12.0-cp36-cp36m-linux_x86_64.whl
p/s: replace pip3 by pip for python2.

# Test tensorflow-gpu:
in terminal: 
>> python3
>> import tensorflow as tf
>> print(tf.contrib.eager.num_gpus())

output:
yyyy-mm-dd hh:mm:ss.nnnnn: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1115] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 2212 MB memory) -> physical GPU (device: 0, name: GeForce GTX 1060 3GB, pci bus id: 0000:01:00.0, compute capability: 6.1)

### Good luck Guys :))) 




### P/S: Bazel should not be updated to version 0.20.0 or higher due to bugs.


