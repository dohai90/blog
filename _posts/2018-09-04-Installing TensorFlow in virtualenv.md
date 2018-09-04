In this post I will talk about installing TensorFlow in virtualenv to prevent conflicting between packages. After installed, we can create jupyter-notebook kernel including virtualenv with TensorFlow. Let's gooooo......^^

## 1. Installing TensorFlow ##

### Install virtualenv ###
    $ sudo apt-get install python-virtualenv

### Create virtualenv and activate it ###
Your can follow this [link](https://www.tensorflow.org/install/install_linux) for more details. I brief here the important commands:

    $ mkdir ~/user/venv
	$ cd ~/user/venv
	$ virtualenv -p python3 ”virtualenv-name”
	$ source /path/to/virtualenv-name/folder/bin/activate

### Upgrade pip in virtualenv ###
	(virtualenv-name) $pip install --upgrade pip

### Install TensorFlow in virtualenv ###
You can install TensorFlow via pip or from source. TensorFlow is easily installed via pip but if you want to optimized TensorFlow to match your hardware specification, you should compile and install it from source.

- Install TensorFlow via pip

CPU version:

	(virtualenv-name) $ pip install tensorflow

GPU version:

	(virtualenv-name) $ pip install tensorflow-gpu



- Install TensorFlow from source

You can follow this [link](https://gist.github.com/Brainiarc7/6d6c3f23ea057775b72c52817759b25c) for more details. I just write down here some important commands tailored for my needs :v.

	(virtualenv-name) $ mkdir ~/user/tools
	(virtualenv-name) $ cd ~/user/tools
	(virtualenv-name) $ git clone https://github.com/tensorflow/tensorflow
	(virtualenv-name) $ cd ~/user/tools/tensorflow
	(virtualenv-name) $ ./configure
	(virtualenv-name) $ bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-mfpmath=both --copt=-msse4.2 --config=cuda //tensorflow/tools/pip_package:build_pip_package
	(virtualenv-name) $ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
	(virtualenv-name) $ pip install --upgrade /tmp/tensorflow_pkg/tensorflow-*.whl

For specific hardware, maybe it does not support instruction sets for AVX2 or FMA, you can leave these instruction sets out of the above command by removing:

	--copt=-mavx2 --copt=-mfma

If you still compile TensorFlow with these options, building process is still successful but the error will occur when you import TensorFlow.

In order to build tensorflow 1.10 from source, we need to install some more packages to prevent compiling issues

	(virtualenv-name) $ pip install keras
	(virtualenv-name) $ pip install keras_applications==1.0.4 --no-deps
	(virtualenv-name) $ pip install keras_preprocessing==1.0.2 --no-deps
	(virtualenv-name) $ pip install h5py==2.8.0

- Create jupyter-notebook kernel

You can follow this [link](https://anbasile.github.io/programming/2017/06/25/jupyter-venv/) for more details.

	(virtualenv-name) $ pip install ipykernel
	(virtualenv-name) $ ipython kernel install --user --name=kernel-name

## 2. Removing virtualenv and jupyter kernel ##
If you don't want to use this jupyter kernel or virtualenv anymore, you can remove them from the system as follows:

Removing jupyter kernel

	$ cd ~/.local/share/jupyter/kernels
	$ rm –rf kernel-name

Removing virtualenv

	(virtualenv-name) $ deactivate
	$ cd ~/user/venv
	$ rm –rf virtualenv-name








 

 



 